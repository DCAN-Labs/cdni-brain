# Analyzing Motion Regressors 

Motion regression is performed on neuroimaging data to reduce motion-related artifacts in the signal. This method focuses on using MATLAB scripts to process the regressor files and identify motion artifacts related to respiration. More information about the scripts can be found on the [Github repo.](https://github.com/DCAN-Labs/movement_regressors_power_plots)

**NOTE:** If you are trying to use this analysis on fmriprep/nibabies outputs, you will need to save your working directory to find the movement regressor files. 

1. Clone the repository by running `git clone https://github.com/DCAN-Labs/movement_regressors_power_plots/`

2. Check the NIfTI headers to find the TR value, which represents the time between succesive scans (it may vary depending on the data)

    - To find with fsl run: 

    `module load fsl`

    `fslinfo sub-***_task-movieDM_bold.nii.gz`

    - To find with workbrench run:

    `module load workbench`

    `wb_command -file-information sub-***_task-movieDM_DM.bold.nii.gz`

3. Find your regressor files in the pipeline derivatives directory 
    
    - If you used the ABCD-HCP pipeline, the regressor files would be located at `processed/abcd-hcp-pipeline/sub-ID/ses-{SESSION}/files/`

    - If you used fmriprep or nibabies, they are located at `work-dir/{pipeline}/sub-ID/ses-SESSION/{pipeline}_wf/single_subject_{ID}_wf/func_preproc_ses_{SESSION}_task_rest_acq_noNORDIC_run_1_echo_1_wf/bold_hmc_wf/normalize_motion/motion_params.txt` (replacing pipeline with the relevant pipeline)

    - For instance, if you're using resting state, select the task-based resting state directories and find this 