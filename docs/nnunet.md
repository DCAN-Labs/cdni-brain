# Running nnUNet Predict on MSI

If you need to create segmentations for a dataset outside of BIBSnet, these are the steps that you need to take. These instructions will allow you to run nnUNet predict with the outputs created by pre-BIBSnet. 

**Setting up the Directory**

First you'll need to create the appropriate folder structure: `data/sub-XXX/ses-XXX/raw`

Then copy over the T1 and/or T2 image to the `raw` folder: `cp <filename> path/to/raw/`

Rename the image ending to 0000.nii.gz and 0001.nii.gz for the T1 and T2 image respectively: `mv <sub-123_ses-456_T1w.nii.gz> <sub-123_ses-456_0000.nii.gz>`

- If you only have a T2 image, rename the file to 0000.nii.gz instead of 0001.nii.gz 


**Step 1: Check Image Orientation**

Now you'll need to reorient to image to standard using fsl.

Move into your working directory where the image lives and run `module load fsl`

To view the image, you can use `fslview_deprecated` or `fsleyes`

To reorient the image, run `fslreorient2std <input_file> <output_filename>`

Example:

```
cd /data/sub-XXX/ses-XXX/raw
module load fsl
fslreorient2std sub-XXX_ses-XXX_0000.nii.gz sub-XXX_ses-XXX_reoriented_0000.nii.gz 
```

**Step 2: Crop Image**

To crop the image run `robustfov -i <input_file> -m <output_matrix> -b <brain size - default 120> -r <output_filename>`

Example: 
`robustfov -i sub-XXX_ses-XXX_reoriented_0000.nii.gz -m ../cropped/roi2full.mat -b 120 -r ../cropped/sub-XXX_ses-XXX_cropped_0000.nii.gz`

**Step 3: Resize Image**

Move into the code folder and [load the labwide miniconda environment](miniconda.md) 

To resize the image run: `python3 resize_images.py <input_folder> <output_folder_filename>`

Example: 
`python3 resize_images.py ../data/sub-XXX/ses-XXX/cropped/ ../data/sub-XXX/ses-XXX/resized/sub-XXX_ses-XXX_resized_0000.nii.gz` 

**Step 4: Prepare and Run sbatch Script**

Go to code folder and edit infer_script.sh according to subject and session you are currently running

Main command: `nnUNet_predict -i <input> -o <output directory> -t <model_number> -m 3d_fullres`

Choose the model number based on what images you have:

552 - T1 and T2 images
514 - T1 only image
515 - T2 only image

Run script:  `sbatch infer_script.sh`
To check your job status: `squeue -al -–me`

**Step 5: Perform Chirality Correction**

Follow these steps to create a LR mask:

- Prepare sbatch script to run command by making a new script starting from `example_run_script_mask.sh`

- The main command is `./LR_mask_registration.sh <resized_inpud> <template image> <template mask>`

    - An example command: `./LR_mask_registration.sh ../data/sub-XXX/ses-XXX/resized/sub-XXX_ses-XXX_resized_0000.nii.gz ./masks/1mo_T2w_acpc_dc_restore.nii.gz ./masks/1mo_template_LRmask.nii.gz`

- Run the sbatch script from the code directory: `sbatch run_script_mask.sh`

- The mask (`LRmask.nii.gz`) will be output in the code directory so you'll need to move it into a `LRmask` folder within the subject directory: `mv LRmask.nii.gz ../data/sub-XXX/ses-XXX/LRmask/`

Follow these steps to perform the correction:

- Prepare sbatch script to run command by making a new script starting from `example_run_script_chirality.sh`

- Before running the command you'll need to load the lab wide miniconda environment

- The main command is `python3 ./correct_chirality.py <segmented image> FreeSurferColorLUT.txt <LR mask created in previous step> <output filename>`

    - An example command: 

    ```
    source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

    python3 ./correct_chirality.py ../data/sub-XXX/ses-XXX/segmented/sub-XXX_ses-XXX_resized.nii.gz ./FreeSurferColorLUT.txt ../data/sub-XXX/ses-XXX/LRmask/LRmask.nii.gz ../data/sub-XXX/ses-XXX/segmented/sub-XXX_ses-XXX_corrected.nii.gz
    ```

- Run the sbatch script from the code directory: `sbatch run_script_chirality.sh`


**Step 6: Re-resize and Un-crop Image**

Change directory to session folder `cd ../data/sub-XXX/ses-XXX`
Both steps use flirt, which is an fsl command. So in case not already done: `module load fsl`  

Re-resize:
Use cropped image (before resizing) as reference to bring back segmentations in original dimensions

Command:  `flirt -applyxfm -in <chirality corrected segmentation> -ref <cropped image> -out <output filename> -init <matrix to be applied - identity matrix ($FSLDIR/etc/flirtsch/ident.mat)> -interp <interpolation method (nearestneighbour)>`

Example Command:

```
flirt -applyxfm -in ./segmented/sub-XXX_ses-XXX_corrected.nii.gz -ref ./cropped/sub-XXX_ses-XXX_cropped_0000.nii.gz -out ./segmented/sub-XXX_ses-XXX_resampled.nii.gz -init $FSLDIR/etc/flirtsch/ident.mat -interp nearestneighbour
```


Un-crop:
Use matrix that was outputted in step 2 (robustfov) to undo cropping and align segmentation to original raw T2 image

Command: `flirt -applyxfm -in <resampled segmentation> -ref <reoriented raw image> -out <output filename> -init <matrix to be applied - output from cropping step (roi2full.mat)> -interp <interpolation method (nearestneighbour)>`

Example: 

```
flirt -applyxfm -in ./segmented/sub-XXX_ses-XXX_resampled.nii.gz -ref ./raw/sub-XXX_ses-XXX_reoriented_0000.nii.gz  -out ./segmented/sub-XXX_ses-XXX_uncropped.nii.gz -init ./cropped/roi2full.mat -interp nearestneighbour
```

**Step 7: Create Derivatives Folder for Nibabies and Create Brain Mask**

Now everything is back in the original space, we can create a brain mask for the data.
This mask is needed in addition to the segmentation as input for the processing pipeline.

Create input folder with the final segmentation file in [right naming convention.](https://nibabies.readthedocs.io/en/latest/faqs.html?highlight=derivatives#leveraging-precomputed-results)

Make folder in derivatives/precomputed, copy final segmentation (‘uncropped’) there and rename it to fit nibabies naming scheme. 

deriv.sh 
Use brain mask python script (module load fsl)
source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh
conda activate cabinet
python3 make_aseg-derived_mask_edited.py <input folder>
Example: python3 make_aseg-derived_mask_edited.py ../data/npad/derivatives/precomputed/sub-4003/ses-031621/anat/
Rename mask to fit naming convention


Further steps for processing: make sure, all inputs are oriented correctly to match the segmentations


Invert T2’s instead of synthetic T1’s:
use fslmaths to multiply T2 intensity by -1 
fslmaths sub-4002_ses-030821_T2w.nii.gz -mul -1 sub-4002_ses-030821_T2w_neg.nii.gz 
Find max intensity
max_intensity=$(fslstats sub-4002_ses-030821_T2w.nii.gz -R)
Add about 5% to it (e.g 2135 -> 2235)
add max intensity (plus a bit) to the inverted image
fslmaths sub-4002_ses-030821_T2w_neg.nii.gz -add 2235 sub-4002_ses-030821_T2w_add.nii.gz
apply mask (previously calculated after segmentation step - in precomputed outputs)
fslmaths sub-4002_ses-030821_T2w_add.nii.gz -mas ../derivatives/precomputed/sub-4002/ses-030821/anat/sub-4002_ses-030821_space-orig_desc-brain_mask.nii.gz sub-4002_ses-030821_T2w_mas.nii.gz

Running Nibabies: 
Copy inverted T1 to anat folder and name it as T1w
Copy .json of T2 and rename it to fit T1
Check if T2 is reoriented2standard





Synthetic T1s:

Copy sub-[]_ses-[]_T2w_acpc.mat
Invert it:
convert_xfm -omat inv_acpc.mat -inverse sub-4002_ses-030821_T2w_acpc.mat 
Apply inverse to synthetic image to put it in same space as raw T2
flirt -applyxfm -in ./test/synth/sub-4002_ses-030821_desc-synthetic_T1w.nii.gz -ref ./raw/sub-4002_ses-030821_0000.nii.gz -out ./test/synth/sub-4002_ses-030821_resampled.nii.gz -init ./test/synth/inv_acpc.mat -interp spline


