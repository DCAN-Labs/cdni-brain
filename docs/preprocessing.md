# 10.5T NHP ABCD BIDS Pipeline Synth

For this pipeline, the data first needs to be converted and properly orientated before being ran.

1. Get DICOMs from s3 bucket (`s3://zlab-nhp`)

1. Convert DICOMs to BIDS and apply NORDIC
    - Use the [Dcm2bids3 NORDIC wrapper](nordic.md) -- needs a pair of 10.5T-specific config files to run, and use `--keep-non-nordic` when calling `nordicsbatch.sh` in the post-op command
    - Confirm the number of noise volumes per run-- for the Z-Lab 10.5T data, this is usually 5 

1. Correct the orientation of the images. **NOTE**: These steps should be correct for data aquired **mid-2021 onward**, older data may be orientated differently. Please check with Thomas (tmadison@umn.edu) if you encounter data that does not match the descriptions below after performing the orientation correction. 

    To correct the fmap and func files:
    - Naviagate into the proper directory: `cd /home/faird/shared/code/internal/utilities/zlab_10p5T_nhp_processing/orientation_fix`

    - Load MATLAB: `module load matlab`

    - Run these commands to fix the orientations

    `for n in  <...>/fmap/*nii.gz; do matlab -nodisplay -nosplash -r "sphinx2headfirstsupineforfmapandfunc('${n}'); exit"; fslreorient2std ${n}; fslswapdim ${n} x y -z ${n}; done`

    `for n in  <...>/func/*nii.gz; do matlab -nodisplay -nosplash -r "sphinx2headfirstsupineforfmapandfunc('${n}'); exit"; fslreorient2std ${n}; fslswapdim ${n} x y -z ${n}; done`

    - Load FSL: `module load fsl`

    - Open an image to check the orientation: `fslview_deprecated <fmap/func-image>`

    - From left to right, the panels should show the sagittal, coronal, and axial view

    - The axis labels (S/I, A/P, L/R) are correct. **L** and **R** labels should be on the **right** and **left** sides of the panel, respectively (i.e. they are on the "wrong" sides) due to the Right-to-Left xorient
    
    - The brain in the center panel should appear "right side up"

    - Check the sform information, should be similar to below output: `fslhd <fmap/fun-image>`
    
                sform_name Scanner Anat
                sform_code 1
                sto_xyz:1 -0.753247 0.000000 0.000000 39.195728 
                sto_xyz:2 0.000000 0.753247 0.000000 -59.425900 
                sto_xyz:3 0.000000 0.000000 0.750000 -3.215199 
                sto_xyz:4 0.000000 0.000000 0.000000 1.000000 
                sform_xorient Right-to-Left
                sform_yorient Posterior-to-Anterior
                sform_zorient Inferior-to-Superior

    For anat images, there shouldn't be any correction needed
    - The "L" and "R" labels should be on the left and right sides respectively (i.e. on their "correct" sides)

    - Check the sform information, which should be similar to below: `fslhd <anat-image>`

                sform_name Scanner Anat
                sform_code 1
                sto_xyz:1 0.500000 0.000000 0.000000 -63.750000 
                sto_xyz:2 0.000000 0.468750 0.000000 -77.436798 
                sto_xyz:3 0.000000 0.000000 0.468750 -67.006302 
                sto_xyz:4 0.000000 0.000000 0.000000 1.000000 
                sform_xorient Left-to-Right
                sform_yorient Posterior-to-Anterior
                sform_zorient Inferior-to-Superior


1. Make T1w brain mask (or use pre-made mask from previous session)

    - Pre-made masks are located at `s3://zlab-nhp/macaque_masks`

    - Use the *dilM_edit.nii.gz* versions which have been manually edited to work better with the pipeline

1. Add IntendedFor fields to fmap jsons

1. Now you are ready to run the pipeline (see [pipelines](pipelines.md) for the command)

## Infant Preprocessing 

Defacing is an important preprocessing step to ensure the anonymity of the subjects. Without defacing, a subjects facial structure could theoretically be reconstructed from a MRI image. Although skull-stripping could also be done to anonymize an image, this can limit what processing can be done to those images and this is especially hard to do in infants, which is why defacing is the standard method of anonymization. 

Defacing is done by creating a rough boxy mask that cuts off the eyes/nose/mouth so that only the brain is left in the image. This mask is created by using an existing atlas that has a mask and registering the subject head to that atlas. A linear transform is then applied to the template mask to create a mask that is in the subject space. 

Usage for a directory that contains a subject folder, and the subject folder contains a T1w image, T2w image, or both:

`python3 /home/feczk001/shared/projects/BCP_deface/code/deface.py /path/to/directory/`

