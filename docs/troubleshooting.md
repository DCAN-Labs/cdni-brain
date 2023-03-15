# Troubleshooting and Image Viewing

For any processing failures, we triage what happened. Most troubleshooting processes will be needed for infant data, as their processing jobs are more prone to errors. When encountering a processing failure, first check to see if it is already documented in the following pipeline dependent links. It is also necessary and efficient to post your error and run command in #questions or the relevant channel on Slack. If you are unsure what your “relevant channel” is on Slack, ask your supervisor or just default to posting in #questions. 


Image viewing is a necessary process to conclude if a pipeline job finished successfully, but it also may be necessary when a processing error has been encountered. There are a few different options for viewing anatomical images. It may be most efficient to first download the images locally and then use ITKsnap to view them. However, when working on MSI, applications such as fslview_deprecated or fsleyes can be used. For viewing functional images, wb_view is ideal (make sure to module load fsl or module load workbench before using these applications). For more information on nifti and cifti files (image file types), refer to the video sessions in the subfolders of [this google drive folder](https://drive.google.com/drive/u/0/folders/1yc3w2zNYVZQvTcCgxKk_j6ecZLoyWiCM).


## infant-abcd-bids-pipeline (DCAN Infant Pipeline)

1. First follow established SOPs to check that the input data is high quality enough to be processed: see _Structural Pre-Processing Quality Assessment of BIDS _under [Quality Control for Infant Data](https://dcanlab.readthedocs.io/en/latest/manualpro/infant/qc/) on the DCAN Labs RTDs 

2. Check the executive summary to spot errors/quality issues in the pipeline outputs: see _Input Data_ and _Post-Processing Quality Assessment from Executive Summary _under [Quality Control for Infant Data](https://dcanlab.readthedocs.io/en/latest/manualpro/infant/qc/) on the DCAN Labs RTDs 

3. For more in-depth quality assessment that involves inspecting intermediary pipeline outputs, see the [Troubleshooting](https://dcanlab.readthedocs.io/en/latest/manualpro/infant/troubleshooting/) section of DCAN Labs RTDs (includes information on inspecting file outputs from PreFreeSurfer, atlas registration, and functional/structural registration)

4. For issues with dense time series files, see [Checking for NaNs and zeros in dtseries](https://docs.google.com/document/d/1dvpISFRuyKDW0Fc9OCu3GO2GOOlMZ-K8w0PPLtD3oH8/edit) to double check for NaNs

## ABCD-BIDS

In the Infant section above, points c-d can be generalized for ABCD troubleshooting as well, but are typically errors more commonly found with infant vs adult processing. For a generalized spot check of abcd-hcp-pipeline outputs for functional data, use workbench view: 

1. For the abcd-hcp-pipeline functional spot check example, our goal is to get a scaffolding of brain activity. In order to see if brain activity is well represented as functional connectivity, we have to look at the BOLD data (a functional output image). This process can give insights as to whether the processing pipeline has an underlying issue that's been overlooked or if the data has been collected poorly. (Note: workbench works best on MSI’s NICE system; with NICE no ssh into an HPC is required.).

    1. Run module load workbench

    2. Navigate to your output directory. This is an example output directory: cd /home/feczk001/shared/data/ABCC_year2_sites/sub-####/
    ses-$$$$/files/MNINonLinear/fsaverage_LR32k

    3. Run wb_view then select _Open _and _Load _for all the files

    4. In the _Montage Selection_ section, switch the selected surfaces from _midthickness_ to_ very_inflated_

    5. Deselect all three of the layers in the _Overlay Toolbox/Layers_ section 

    6. Go to _File _and _Open File_, switch the _Files of type_ section to _CIFTI - Dense Data Series Files (*.dtseries.nii)_

    7. Navigate up one directory then down into the _Results _folder

    8. Select a _task_rest dtseries.nii_ and open it 
    
    9. Go to _File _and _Open File, _and navigate to the parent folder (feczk001/shared/). From here open _ROI_sets_, _Surface_schemes_, _Human_, _Gordon_, and _fsLR_
    
    10. Switch the _Files of type_ section to _CIFTI - Dense Label Files (*.dlabel.nii)_
    
    11. Open the _Gordon.networks _file
    
    12. Back in the _Overlay Toolbox/Layers/File _section, switch the first two files to the _Gordon.networks _file and the _dyconn - task-rest_ file, and check the box under _On_ for those two files
    
    13. For the _Gordon.networks _file, click the wrench icon under _Settings_ and in the _Labels/Drawing Type _section, select _Outline Label Color_, then select _Close_.

        From here, select points on the brain images to see if the more yellow colors in the _dyconn_ file correspond to the areas outlined by the _Gordon.networks_ file. Focus on distinct networks like the Visual, Motor, and Default Mode. If the seed maps (_dyconn _file) don’t correspond well with the networks, there may be an issue with the data acquisition or the pipeline. For a more in-depth look at this process, see [here](https://umn.app.box.com/file/980329914631). 

2. For data processed with the abcd-hcp-pipeline, the final outputs will always be in the MNINonLinear/fsaverage_LR32k folder, i.e. atlas transformed outputs.

3. Below is an example of what wb_view should look like. The order of the networks seen below are Default Mode, Motor and Visual. The overlays for each all look fairly accurate, as the yellow activation areas correspond to the network region where the voxel was selected. Important to note that one will need to select multiple voxels within a network region in order to identify whether the BOLD activation truly overlays properly with the networks.


## NiBabies and fMRIprep

For a comprehensive document on troubleshooting nibabies and fmriprep errors, [see here](https://docs.google.com/document/u/0/d/16qSEPV1_FHOHBq2eJOuZLqISv-0zCbpOJQ7HesEQCv4/edit).

## XCP-D

For common troubleshooting processes and a deeper dive into the utilization of XCP-D, [see here](https://umn.app.box.com/folder/149404140292?s=3gmexhky3rtxafdo118rh1c0c2x35jl6).

## S3 Wrappers


**If there are run files:**


Errors are common when running preliminary tests on s3 wrappers. In order to troubleshoot these errors, the first step is to grab an[ interactive session](slurm-params.md#srun-immediately-run-a-command-using-the-specified-compute-resources). Next, run the content of one run file by copying and pasting the contents into the terminal step-by-step in order to see the outputs of each command in real time, which will uncover the step in which the error is occurring. Once the error is reproduced, incorporate the fix, then rerun make_run_files.sh and a test subject for the submitter script.


**If there are not run files:**


The issue should be regarding the paths to the files in make_run_files.sh. With make_run_files.sh opened, copy and paste the contents of the file into the terminal in order to see if the conditional statements match the necessary paths of the files that need to be included. 