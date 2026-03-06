# Neuroimaging Softwares

CDNI commonly uses a few different softwares for visualizing and manipulating MRI data. The most commonly used softwares are Workbench and FSL. This page will walk through some of the common commands and uses of each software.

## Workbench

[Connectome Workbench](https://humanconnectome.org/software/workbench-command) is an open source visualization and discovery tool used to view and manipulate neuroimaging data. To use on MSI, run `module load workbench`. Workbench is primarily used to work with CIFTIs and surface data (GIFTIs) and can also be used with volume data (NIfTIs). 

<div class="admonition note">
    <p class="first admonition-title">Note</p>
    <p class="last">
        Some CDNI tools might require older versions of Workbench. The MSI default is 2.0.1 (as of Summer 2025). If you are running into unexpected Workbench errors, try loading an older version of Workbench (like 1.4.2 or 1.5.0) to see if that fixes the issue.
    </p>
</div>

There are two primary commands for workbench `wb_view` and `wb_command`.

- `wb_view` will open an interactive GUI used for inspecting images. This is often used during troubleshooting and QC.

- `wb_command` has a collection of commands used to print out file information, perform calculations, merge images, and other manipulations. 

### Viewing Images

`wb_view path/to/image` will open the image in a GUI. This GUI can be super slow as it requires a lot of resources to run. For a chance to increase performance speed, grab an srun with more memory. 

This GUI can be used to visually inspect many data types, including:

- CIFTI data (.dlabel, .dscalar, .dtseries, .pconn, etc)
- Surface data (.surf.gii files)
- Volume data (.nii/.nii.gz files) 

Connectome Workbench has created some very useful tutorials on data visualization. They have a [Volume Viewing Tutorial](https://www.youtube.com/watch?v=TwMGQmltVrU) and a [Surface Viewing Tutorial](https://www.youtube.com/watch?v=bl_jCRHOoUI).

### Manipulating data

Here are some useful commands that are called with `wb_command <command> <options>`. To see the full usage of any command, run `wb_command <command>` with no inputs. Running `wb_command -list-commands` will show all of the possible workbench commands and a brief description of their function. 

Here is a list of commonly used commands:

`-cifti-average`
- Create a row of average data from a CIFTI 

`-cifti-correlation`
- Create dconn from dtseries

`-citfi-create-dense-scalar` 
- Create dscalar from nifti

`-cifti-create-label`
- Create dlabel from gifti

`-cifti-math <expression> <output> -var <variable_name> <cifti_file>` 
- Perform math expression on each matrix element of a CIFTI

`-cifti-reduce <input_cifti> <operation> <output_cifti>` 
- Get certain stats from CIFTI rows/columns (i.e. max/min vals, sum/product, mean/stdev/median, etc)

`-cifti-parcellate` 
- Turn dense CIFTI into a parcellated CIFTI

`-cifti-separate`
- Separates a CIFTI into separate GIFTIs/NIFTIs

`-cifti-smoothing`
- Create smoothed CIFTI with gaussian kernel

`-file-information <filepath>`
- List information about the contents of a file, like its dimensions and header information

`-metric-math <expression> <output> -var <variable_name> <cifti_file>`
- Perform math expression on each vertex of a GIFTI file

`-metric-reduce <input_gifti> <operation> <output_gifti>`
- Get certain stats from GIFTI rows/columns (i.e. max/min vals, sum/product, mean/stdev/median, etc)

`-metric-stats` 
- Print stats from GIFTI columns (i.e. max/min vals, sum/product, mean/stdev/median, etc)

`-surface-information <filepath>` 
- Get metadata from .surf.gii (i.e. vertices, triangles, bounding box, and spacing)

`-volume-label-import` 
- Create label volume from volume file (assign names/colors to indices/indexes/ROIs of segmentation)

`-volume-to-surface-mapping <volume_file> <surface_file> <output_file>` 
- Project volume data to surface

## FSL

[FSL](https://fsl.fmrib.ox.ac.uk/fsl/docs/#/) is a library of visualization and analysis tools for fMRI, MRI and diffusion brain imaging data. To use on MSI, run `module load fsl`. FSL is primarily used to work with volume data (NIFTIs).

### Visualization

FSLeyes is the FSL tool to visualize data. FSL is primarily for use with volume data (.nii / .nii.gz) and is commonly used to view brain segmentations, since they have a better color palette to choose from compared to Workbench. FSLeyes is also generally quicker to use than wb_view. You can open FSLeyes after you load the fsl module and run `fsleyes <image>`. 

### Manipulation

`cluster`
- Detects contiguous chunks of voxels, used for cleaning brainmasks

`flirt` and `fnirt`
- Linear and non-linear image registration

`fsledithd` 
- Edit nifti file header

`fslhd` and `fslinfo` 
- Print header information. fslhd is more verbose

`fslmaths` 
- Arthimatic manipulations, apply brainmask, subtract an image from another, binarize, make masks, realigning, etc

`fslmerge` 
- Concat images across space/time

`fslroi` 
- Extract part of image across space/time

`fslstats`
- Get/compute image statistics

`mcflirt` 
- Motion correction of BOLD data

## AFNI

[AFNI](https://afni.nimh.nih.gov/) is an toolbox of multiple programming languages that can perform many of the same data manipulations as FSL. To use on MSI, run `module load afni` and open the GUI with `afni`.

## ITK-SNAP 

[ITK-SNAP](https://www.itksnap.org/pmwiki/pmwiki.php) is a visualization software primarily used for segmentations but is also used to visualize raw DICOMs. It can be used to manually edit segmentations (common for infants and atypical brains) and is also often used for QC image viewing. To use on MSI, run `module load itk-snap` and open the GUI with `itksnap`. 

## MRIcroGL

[MRIcroGL](https://www.nitrc.org/projects/mricrogl/) is another visualization tool for raw DICOM images. To use on MSI, run `module load mricrogl` and open the GUI with `MRIcroGL`

## ANTS

[ANTS](https://github.com/ANTsX/ANTs) is a neuroimaging software that is primarily used with processing pipelines for anatomical pre-processing (denoising, bias field correction, registration, etc). Sometimes you will need to do these steps outside of the processing pipeline. 

Here are some common commands:

`atropos`
- Segmentation tool that can be used for brainmasking (primarily used in infants)

`denoiseImage`
- Denoise an anatomical image

`N4BiasFieldCorrection`
- Apply bias field correction to fix signal inhomogenaity in anatomical images

`antsApplyTransform`
- Apply ANTs transform

`antsRegistration`
- High-quality brain registration algorithm

## NiBabel

[NiBabel](https://nipy.org/nibabel/) is a Python package used for working with neuroimaging data in Python. This is a special package that needs to be installed in your environment to use. It is one of the packages installed in the lab-wide miniconda environment and within several conda environments. See our [miniconda page](miniconda.md) for more information. 

For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
