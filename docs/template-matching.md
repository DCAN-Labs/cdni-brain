# Template Matching

Template matching is a method of network mapping that leverages commonly observed networks that have been previously observed in a group average to accelerate the community detection process. Individual networks are identified in a 3-step process:

* **Run a community detection of your choosing on an average connectivity matrix**: In Hermosillo et al. 2022, we used networks defined from an average connectivity matrix created from 120 healthy adults. Infomap community detection was conducted on the average matrix (See Gordon et al. 2017 for details).

* **Generate a template of networks**: Using the networks commonly observed in a group, we can create a set of network templates that are specific to the training group.

* **Match connectivity of each voxel**: Using a dense connectivity matrix from a subject in an independent test group, we correlate each voxel, or seed, to each of the network templates. Voxels are assigned to the network with the highest spatial similarity to the network template.

The code can be found on MSI at `/home/faird/shared/code/internal/analytics/compare_matrices_to_assign_networks`

## Usage

There are 4 files needed to run template matching which are output from XCP-D:

- func/sub-{SUB}_ses-{SES}_task-{TASK}_space-fsLR_den-91k_desc-denoised_bold.dtseries.nii
- func/sub-{SUB}_ses-{SES}_task-{TASK}_desc-abcc_qc_power_2014_FD_only.mat
- anat/sub-{SUB}_ses-{SES}_space-fsLR_den-32k_hemi-L_desc-hcp_midthickness.surf.gii
- anat/sub-{SUB}_ses-{SES}_space-fsLR_den-32k_hemi-R_desc-hcp_midthickness.surf.gii

In older versions of XCP-D (< 0.8.0) look for these func filenames instead:

- sub-{SUB}_ses-{SES}_task-{TASK}_space-fsLR_den-91k_desc-**interpolated**_bold.dtseries.nii
- sub-{SUB}_ses-{SES}_task-{TASK}_desc-**dcan**_qc_power_2014_FD_only.mat

The dtseries will need to be interpolated in order to fill in 0 values along the midline produced by fMRIprep. Use this script for versions of XCP-D >= 0.8.0: `/home/faird/shared/code/internal/utilities/interpolate_timeseries_from_xcpd9/`

- You'll need to provide the subject ID, session ID, task, and path to where the dtseries is as arguments to the script. 

- If you're using an older version, copy this script somewhere else and adjust the name of the dtseries.

Other required inputs are the TR values (check a BIDS json for a func run to find this) and the desired FD threshold (generally 0.2).

Here is an example sbatch file to run template matching. This job generally takes no longer than 2 hours with 120GB of memory. 

```
SUB="5004"
SES=1
TEMPLATE="ABCD"

MASK_PATH='none'
if [ ${TEMPLATE} == "elabe" ] ; then
  TEMPLATE_PATH='/home/faird/shared/projects/infant_probabilistic_atlas_eLABE/TM_analysis/template/outputs/seedmaps_list_dtseries_SurfOnly_all_networksZscored.mat'
elif [ ${TEMPLATE} == "ABCD" ] ; then
  TEMPLATE_PATH='/home/rando149/shared/projects/ABCD_template_maker/seedmaps_from_template2.0/ABCD_scan_seedmap/seedmaps_subs_withsmoothed_dtseries_n141_all_networksZscored.mat'
elif [ ${TEMPLATE} == "infant" ] ; then
  TEMPLATE_PATH='/home/faird/shared/code/internal/analytics/compare_matrices_copy_to_merge_from/support_files/infant_surface_template/seedmaps_UCI_smoothed_dtseries_all_networksZscored.mat'
fi

OUTDIR=/path/to/existing/output/dir/
SURF_ONLY=0 #calculate surface only
ALREADY_SURF_ONLY=0 #only needed if surf_only=1, will scrub out subcortical data
TR=1.761
FD_threshold=0.2
INFOMAP=0 #run infomap if 1

/home/miran045/shared/projects/WashU_Nordic/code/twins_mapping_wrapper_washu_nordic.sh \
${TR} \
${FD_threshold} \
/path/to/subject_files/sub-${SUB}/sub-${SUB}_ses-${SES}_task-restMENORDICrmnoisevols_space-fsLR_den-91k_desc-denoised_bold_spatially_interpolated.dtseries.nii \
/path/to/subject_files/sub-${SUB}/sub-${SUB}_ses-${SES}_task-restMENORDICrmnoisevols_desc-abcc_qc_power_2014_FD_only.mat \
/path/to/subject_files/sub-${SUB}/sub-${SUB}_ses-${SES}_space-fsLR_den-32k_hemi-L_desc-hcp_midthickness.surf.gii \
/path/to/subject_files/sub-${SUB}/sub-${SUB}_ses-${SES}_space-fsLR_den-32k_hemi-R_desc-hcp_midthickness.surf.gii \
sub-${SUB}_ses-${SES}_recode_template \
${OUTDIR} \
${TEMPLATE_PATH} \
${SURF_ONLY} \
${ALREADY_SURF_ONLY} \
${OUTDIR} \
/path/to/subject_files/sub-${SUB}/sub-${SUB}_ses-${SES}_task-restMENORDICrmnoisevols_space-fsLR_den-91k_desc-denoised_bold_spatially_interpolated.dtseries.nii \
${MASK_PATH} \
${INFOMAP}
```

Version-controlled code and documentation can be found on [GitHub.](https://github.com/DCAN-Labs/compare_matrices_to_assign_networks)


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).
