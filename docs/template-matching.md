# Template Matching

Template matching is a method of network mapping that leverages commonly observed networks that have been previously observed in a group average to accelerate the community detection process. Individual networks are identified in a 3-step process:

* **Run a community detection of your choosing on an average connectivity matrix**: In Hermosillo et al. 2022, we used networks defined from an average connectivity matrix created from 120 healthy adults. Infomap community detection was conducted on the average matrix (See Gordon et al. 2017 for details).

* **Generate a template of networks**: Using the networks commonly observed in a group, we can create a set of network templates that are specific to the training group.

* **Match connectivity of each voxel**: Using a dense connectivity matrix from a subject in an independent test group, we correlate each voxel, or seed, to each of the network templates. Voxels are assigned to the network with the highest spatial similarity to the network template.

There are two 'versions' of template matching used in CDNI. One, written by Robert Hermosillo, is based primarily in MATLAB and the other, developed by Kate Godfrey, is based in Python. They both perform the same base function, with the MATLAB version having more customization options. The MATLAB version is helpful for standard use cases and running lots of data, as it is [SLURM wrapper](wrappers.md) friendly. The Python version is easier to modify for special use cases and not yet wrapper friendly. 

## MATLAB Version

Find the [version-controlled code and documentation](https://github.com/DCAN-Labs/compare_matrices_to_assign_networks) on GitHub.

The code can be found on MSI at `/home/faird/shared/code/internal/analytics/compare_matrices_to_assign_networks`

### Required Inputs

There are 4 files needed (which you can find in the XCP-D output directories for the subject):

- func/sub-{SUB}_ses-{SES}_task-{TASK}_space-fsLR_den-91k_desc-denoised_bold.dtseries.nii
- func/sub-{SUB}_ses-{SES}_task-{TASK}_desc-abcc_qc_power_2014_FD_only.mat
- anat/sub-{SUB}_ses-{SES}_space-fsLR_den-32k_hemi-L_desc-hcp_midthickness.surf.gii
- anat/sub-{SUB}_ses-{SES}_space-fsLR_den-32k_hemi-R_desc-hcp_midthickness.surf.gii

In older versions of XCP-D (< 0.8.0) look for these func filenames instead:

- sub-{SUB}_ses-{SES}_task-{TASK}_space-fsLR_den-91k_desc-**interpolated**_bold.dtseries.nii
- sub-{SUB}_ses-{SES}_task-{TASK}_desc-**dcan**_qc_power_2014_FD_only.mat

The dtseries will need to be interpolated in order to fill in 0 values along the midline produced by fMRIprep. Use this script for versions of XCP-D >= 0.8.0: `/home/faird/shared/code/internal/utilities/interpolate_timeseries_from_xcpd9/`

- Script usage: `/path/to/interpolate_timeseries_from_xcpd9 <subjID> <sesID> /path/to/dtseries_folder/` 

- If you're using an older version, copy this script to your working dir and adjust the name of the dtseries.

You will also need the TR value (check a BIDS json for a func run to find this) and the desired FD threshold (generally 0.2).

### Example Command

Here is an example sbatch file to run template matching with data ran through NORDIC. This job generally takes no longer than 2 hours with 120GB of memory. **PLEASE NOTE** that the output directory needs to already exist!

```
SUB="A1B2"
SES=1

TEMPLATE_PATH='/home/rando149/shared/projects/ABCD_template_maker/seedmaps_from_template2.0/ABCD_scan_seedmap/seedmaps_subs_withsmoothed_dtseries_n141_all_networksZscored.mat'
OUTDIR=/path/to/existing/output/dir/
SURF_ONLY=0 #calculate surface only
ALREADY_SURF_ONLY=0 #only needed if surf_only=1, will scrub out subcortical data
TR=1.761
FD_threshold=0.2
INFOMAP=0 #run infomap if 1

/home/faird/shared/code/internal/analytics/compare_matrices_to_assign_networks/twins_mapping_wrapper_washu_nordic.sh \
${TR} \
${FD_threshold} \
/path/to/input_files/sub-${SUB}_ses-${SES}_task-restMENORDICrmnoisevols_space-fsLR_den-91k_desc-denoised_bold_spatially_interpolated.dtseries.nii \
/path/to/input_files/sub-${SUB}_ses-${SES}_task-restMENORDICrmnoisevols_desc-abcc_qc_power_2014_FD_only.mat \
/path/to/input_files/sub-${SUB}_ses-${SES}_space-fsLR_den-32k_hemi-L_desc-hcp_midthickness.surf.gii \
/path/to/input_files/sub-${SUB}_ses-${SES}_space-fsLR_den-32k_hemi-R_desc-hcp_midthickness.surf.gii \
sub-${SUB}_ses-${SES}_recode_template \
${OUTDIR} \
${TEMPLATE_PATH} \
${SURF_ONLY} \
${ALREADY_SURF_ONLY} \
${OUTDIR} \
/path/to/input_files/sub-${SUB}_ses-${SES}_task-restMENORDICrmnoisevols_space-fsLR_den-91k_desc-denoised_bold_spatially_interpolated.dtseries.nii \
${MASK_PATH} \
${INFOMAP}
```

Depending on what data you're running, you might need to change the template used. The scripts in the given code path contain this collection of templates to use.

```
TEMPLATE="ABCD"
if [ ${TEMPLATE} == "elabe" ] ; then
  TEMPLATE_PATH='/home/faird/shared/projects/infant_probabilistic_atlas_eLABE/TM_analysis/template/outputs/seedmaps_list_dtseries_SurfOnly_all_networksZscored.mat'
elif [ ${TEMPLATE} == "ABCD" ] ; then
  TEMPLATE_PATH='/home/rando149/shared/projects/ABCD_template_maker/seedmaps_from_template2.0/ABCD_scan_seedmap/seedmaps_subs_withsmoothed_dtseries_n141_all_networksZscored.mat'
elif [ ${TEMPLATE} == "infant" ] ; then
  TEMPLATE_PATH='/home/faird/shared/code/internal/analytics/compare_matrices_copy_to_merge_from/support_files/infant_surface_template/seedmaps_UCI_smoothed_dtseries_all_networksZscored.mat'
fi
```

There is also a modified run script that will keep the dconn that is created as an intermediate output. This is sometimes helpful for further analysis. `/home/faird/shared/code/internal/analytics/compare_matrices_to_assign_networks/twins_mapping_wrapper_washu_nordic_keepall.sh` 


## Python Version

The code can be found on MSI at `/home/faird/shared/code/internal/analytics/compare_matrices_python`

One of the biggest differences in how the python version is run is that it assumes you've already made the dconn from the subjects' dtseries. This version also doesn't do any motion censoring, so if that is needed for your data, you need to scrub the dconn yourself. 

For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).