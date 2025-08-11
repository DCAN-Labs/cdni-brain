# Neuroimaging Datatypes

This page will explain the many different datatypes involved in MRI research, from the raw images acquired from the scanner to the processing pipeline outputs.

The typical sequences taken at an MRI visit are structural scans (a T1w and/or a T2w), fieldmaps, and functional runs (rest or task). Structural scans provide information about the anatomy of the brain. Fieldmaps are used for distortion correction. They are a pair of images taken in the two directions of how the scanner moves to take an image, normally either Left to Right and Right to Left (LR/RL) or Anterior to Posterior and Posterior to Anterior (AP/PA). These images are used to correct for the "lag" that happens when scanning, which causes the front or back of the brain to become distorted. The functional runs collect the BOLD signal of the brain and can be used to investigate brain connectivity. Diffusion weighted images (DWI) are also sometimes collected for investigate the tractography of the brain. 

## DICOM

DICOMs are the standard file format that is collected from the scanner. Fun fact, DICOM stands for Digital Imaging and Communications in Medicine. DICOM files have a `.dcm` ending. These files store the imaging data as well as header information that indicates information about the scanning participant and scan sequence. You can access the header information with `dcmdump`, WHICH YOU CAN ACCESS ON MSI SOMEHOW or you can use the `pydicom` package in Python. 

These raw files take up tons of space and are complicated to work with, as a single DICOM is one frame of the MRI scan. This is part of the reason they need to be converted to the NIfTI format. We don't often work directly with DICOMs, except maybe looking at them with [ITK-SNAP](workbench.md#itk-snap) to check the data quality.

## NIfTI

NIfTI is the standard file format for working with MRI data. Another fun fact, NIfTI stands for Neuroimaging Informatics Technology Initiative. This format compiles all of the DICOMs from a run into one file and significantly decreases the size of the data by storing it more efficiently. NIfTIs store volumetric data. These files have a `.nii` or `.nii.gz` ending. 

## GIfTI

GIfTI is another standard file format for surface MRI data. They are Geometry NIfTI files and store the surface mesh data. These files have a `.gii` ending. These filetypes are outputs from processing pipelines. Some example filetypes include 

* `.shape.gii` - can store information such a cortical thickness and curvature
* `.surf.gii`- stores the different surface information, like midthickness, pial, and white surfaces
* `.label.gii`- 
* `.func.gii` - 

## CIfTI

CIfTI is a sort of expanded NIfTI that can hold more data. These files store volumetric and surface data. These file types are outputs from processing pipelines and analysis methods. Since these are basically just expanded NIfTI files, they also have `.nii` endings. There are two general types of CIfTI files, dense files and parcellated files. Dense files have data for the entire brain and are used for whole brain connectivity analyses. Parcellated files have the data split out by a parcellation file and are used for analyses looking at average ROI/parcel connectivity. Some example filetypes include 

* `.dtseries.nii`/`.ptseries.nii` - raw timeseries data, every column is a TR and every row is the signal at that grayordinate/parcel 
* `.dscalar.nii`/`.pscalar.nii` - 
* `.dconn.nii`/`.pconn.nii` - correlation matrix of the dtseries data, how every grayordinate is associated with every other grayordinate 
* `.dlabel.nii`/`.plabel.nii` - 

You can read more about CIfTI files (and how to work with them in R) in this [ciftiTools paper](https://www.sciencedirect.com/science/article/pii/S1053811922000076).

## BIDS

[BIDS](https://bids-specification.readthedocs.io/en/stable/introduction.html) is a standard directory structure and filenaming convention used in MRI research. It stands for Brain Imaging Data Structure. The BIDS standard allows researchers and practitioners to easily navigate and understand the contents of the dataset by providing a consistent organization of the data. When we [convert DICOMs to NIfTIs](dcm2bids.md), we also rename the files to be in the BIDS standard, as this is what most pipelines and tools will expect. When we have converted a subjects raw data to NIfTIs in this format, we will refer to that data as the BIDS data. Processing pipeline outputs are also BIDS valid, although we normally just refer to them as derivatives. 

Below is an examples of a BIDS directory structure for a participant of the ABCD study.

```
|--dataset_description.json
|--README
|--CHANGES
|--participants.tsv
|--task-(MID|nback|rest|SST)_bold.json
|--sub-SUBID
   |--ses-SESID
      |--anat
         |--sub-SUBID_ses-SESID(_rec-normalized)_(T1w|T2w).json
         |--sub-SUBID_ses-SESID(_rec-normalized)_(T1w|T2w).nii.gz
      |--dwi
         |--sub-SUBID_ses-SESID_dwi.bval
         |--sub-SUBID_ses-SESID_dwi.bvec
         |--sub-SUBID_ses-SESID_dwi.json
         |--sub-SUBID_ses-SESID_dwi.nii.gz
      |--fmap
         |--sub-SUBID_ses-SESID_(acq-dwi_)dir-(AP|PA)_run-0#_epi.json
         |--sub-SUBID_ses-SESID_(acq-dwi_)dir-(AP|PA)_run-0#_epi.nii.gz
      |--func
         |--sub-SUBID_ses-SESID_task-(MID|nback|rest|SST)_run-0#_bold.json
         |--sub-SUBID_ses-SESID_task-(MID|nback|rest|SST)_run-0#_bold.nii.gz
```

## Data Workflow

MRI data needs to be processed for a variety of reasons. First of all, the data collected from the scanner has lots of "background noise" that needs to get filtered out. There are also motion artifacts from the participants moving in the scanner (just breathing is enough movement to cause an artifact!) and distortion due to the magnetic field of the scanner and the direction that the scanner takes the image. Processing the data "cleans" it up and also extracts meaningful information about the brain structure and function. It is important for the data the be processed in a replicable way, so that the data can be compared and used for subsequent analyses. This is why we use standardized [pipelines](pipelines.md) for our processing. Outlined below are the general steps for MRI data processing. 

1. [DICOM to BIDS](dcm2bids.md) - convert raw DICOMS to NIFTIs using the BIDS format
2. [NORDIC](nordic.md) - a thermal denoising algorithm to reduce unstructured noise
3. BIBSnet - for infant processing, creates better brain segmentation and mask to feed into infant processing pipeline
4. Pre-processing (ABCD-BIDS or NiBabies/fMRIprep) - anatomical processing (intensity correction, alignment, tissue segmentation, normalization, surface reconstruction) and functional processing (brain mask, motion and distortion correction) with outputs mapped to a standard atlas space
5. Post-processing (XCP-D) - further functional processing to generate denoised BOLD images, parcellated time series, and functional connectivity matrices
6. Further analyses - [template matching](template-matching.md) is a popular next step but there are many options

This is only a brief overview of the general steps of MRI data processing. We have dedicated pages to almost each of these steps to explain in further detail how to do each step, including a [processing workflow section](processing-sop.md#processing-workflow) to explain how to process data on MSI. 

For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).