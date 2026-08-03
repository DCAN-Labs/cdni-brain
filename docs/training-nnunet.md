# Manual Training nnUNet

<div class="admonition attention">
    <p class="first admonition-title">Attention</p>
    <p class="last">
        This page is still under construction. Please keep that in mind when reading.
    </p>
</div>

This page provides the code on how to train segmentation models manually outside of the streamlined GUI. You will need to grab [a srun](slurm-params.md#srun) to run these commands. You will also need to [load the labwide miniconda environment](miniconda.md) and activate the `SynthSeg-fixed-perms` conda environment. You will also need to have stable local copies of these two github repositories: [dcan-nn-unet](https://github.com/DCAN-Labs/dcan-nn-unet) and [SynthSeg](https://github.com/DCAN-Labs/SynthSeg). Currently, there are copies of all necessary code available here: `/projects/standard/faird/shared/code/stable/utilities/segmentation_model_training/`. These copies will be used for the examples below (set as the variable `$stable_code_path=/projects/standard/faird/shared/code/stable/utilities/segmentation_model_training/`), but they are stable versions and should **not** be altered. Any testing should be done on your own local copies of the code. Run the set of commands below after setting up your dataset on `scratch.global` with the following example data structure:

```
    scratch.global/some_general_name/
    ├── nnUNet_raw_data_base
    │   ├── nnUNet_raw_data
    │   │   ├── Task###
    │   │   │   ├── imagesTr
    │   │   │   ├── imagesTs
    │   │   │   ├── labelsTr
    │   │   │   ├── labelsTs
    │   ├── nnUNet_preprocessed
    │   ├── nnUNet_cropped_data
```

**Resize images**

```
source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh
conda activate SynthSeg-fixed-perms
cd ${stable_code_path}
```

Rename current subfolders under Task### to "Old_$$$$$" (ex: Old_imagesTr, Old_imagesTs, etc.), and make new empty copies with the original name to be used as destination folders.

```
python ${stable_code_path}/dcan-nn-unet/dcan/img_processing/resize_images.py /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/Old_labelsTr/ /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/labelsTr/	
```

**Create min/maxes (hard coded paths):**

```
ssh -Y agate

srun --time=96:00:00 --mem=128GB --tmp=40gb -p msismall -A $account --x11 --pty bash

source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

conda activate SynthSeg-fixed-perms

cd ${stable_code_path}/SynthSeg

export PYTHONPATH=${PYTHONPATH}:${stable_code_path}/SynthSeg/

export PYTHONPATH=${PYTHONPATH}:${stable_code_path}/SynthSeg/SynthSeg/

python ./SynthSeg/dcan/ten_fold_uniformity_estimation_test.py
```

***Running SynthSeg:***

```
ssh -Y agate

srun --time=96:00:00 --mem=256GB --tmp=80gb -p msismall -A $account --x11 --pty bash

cd ${stable_code_path}/SynthSeg

export PYTHONPATH=${PYTHONPATH}:/projects/standard/faird/lundq163/SynthSeg/

export PYTHONPATH=${PYTHONPATH}:/projects/standard/faird/lundq163/SynthSeg/SynthSeg/

python ./SynthSeg/dcan/image_generation_for_all_ages.py /scratch.global/lundq163/nnUNet_HBCD/nnUNet_raw_data_base/nnUNet_raw_data/Task528/ /scratch.global/lundq163/nnUNet_HBCD/nnUNet_raw_data_base/nnUNet_raw_data/Task528/SynthSeg_generated/ /projects/standard/faird/lundq163/SynthSeg/data/labels_classes_priors/dcan/uniform/528/mins_maxes.npy 2000 --distribution="uniform"

python ./SynthSeg/dcan/image_generation_for_all_ages.py /scratch.global/lundq163/nnUNet_HBCD/nnUNet_raw_data_base/nnUNet_raw_data/Task527_HBCD/ /scratch.global/lundq163/nnUNet_HBCD/nnUNet_raw_data_base/nnUNet_raw_data/Task527_HBCD/SynthSeg_generated/ /projects/standard/faird/lundq163/SynthSeg/data/labels_classes_priors/dcan/uniform/527/mins_maxes.npy 1000 --distribution="uniform"

python ./SynthSeg/dcan/image_generation_for_all_ages.py /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task700_T1_T2_Fold0/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task700_T1_T2_Fold0/SynthSeg_generated/ /projects/standard/faird/lundq163/SynthSeg/data/labels_classes_priors/dcan/uniform/mins_maxes_fold_0.npy 1000 --distribution="uniform"
```

Running one fold of nnUNet:
First - copy over synthseg images to training folders:

```
python copy_over_augmented_image_files.py /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/SynthSeg_generated/images/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/imagesTr/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/labelsTr/

python copy_over_augmented_image_files.py /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/SynthSeg_generated/labels/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/imagesTr/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/labelsTr/

cd /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/
mv ./imagesTr/*_SynthSeg_generated_0000.nii.gz ./labelsTr/ -v
mv ./imagesTr/*_SynthSeg_generated_0001.nii.gz ./labelsTr/ -v
ls ./imagesTr/ | wc -l
ls ./labelsTr/ | wc -l
rm SynthSeg_generated/ -r
```
imagesTr should have 18162, and labelsTr should have 9081, and then SynthSeg_generated can be removed


Create dataset json for nnunet plan and preprocess:

```
cd /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task700/

export PYTHONPATH=${PYTHONPATH}:/projects/standard/faird/lundq163/dcan-nn-unet/
export nnUNet_raw_data_base="/scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/"
export nnUNet_preprocessed="/scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_preprocessed/"
export RESULTS_FOLDER="/projects/standard/feczk001/shared/data/nnUNet/nnUNet_raw_data_base/nnUNet_trained_models"

python /projects/standard/faird/lundq163/dcan-nn-unet/dcan/dataset_conversion/create_json_file.py TaskXXX

export PYTHONPATH=${PYTHONPATH}:/projects/standard/faird/lundq163/dcan-nn-unet/dcan/

python /projects/standard/faird/lundq163/dcan-nn-unet/dcan/dataset_conversion/fix_json_file.py ./dataset.json ./dataset2.json
'next remove comma after vermis in dataset2, then delete dataset and save dataset2 as dataset'
```

Run plan and preprocess:

```
cd /projects/standard/faird/shared/code/internal/nnUNet/slurm_scripts/70X
sbatch NnUnet_plan_and_preprocess_agate.sh
```


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).

# Automated Training nnUNet

This repository [Github](https://github.com/DCAN-Labs/Seg-Model-Creation-GUI.git) contains the instructions and code needed to run the segmentation model training pipeline through the GUI.

## Access Requirement

Before using this pipeline, you must be a member of the **faird** group on MSI.  
If you do not have access, you will not be able to run the required environment or access the necessary project directories.

## Clone the Repository

Clone the GUI repository and follow instructions posted to the README.md within the repo:

```bash
git clone https://github.com/DCAN-Labs/Seg-Model-Creation-GUI.git
```

For questions or issues, please contact the development team: @Emoney and @Kenevan-Carter

