# Templates used by DCAN-infant pipeline

We have a range of age-specific atlas folders located on MSI under `/home/faird/shared/code/internal/pipelines/age_specific_atlases` which include the following age bins (in months of age):

00-02 \
02-05 \
08-11 \
11-14 \
14-17 \
17-21 \
21-27 \
27-33 \
33-44 \
44-60 

Note that the default templates included in the pipeline container ([/global/templates](https://github.com/DCAN-Labs/dcan-infant-pipeline/tree/main/global/templates)) correspond to the `00-02` month old folder and alternative template folders can be fed into the pipeline by binding the template folder like so: `-B </local/template/folder/path>:/opt/pipeline/global/templates`

## The template folders contain the following:

**INFANT_MNI template files** \
These are NIH age-specific atlases in MNI space called INFANT_MNI downloaded from http://nist.mni.mcgill.ca/?p=1005. Former documentation indicates that the folders were created after downloading using the script `/home/groups/brainmri/infant/NIH_ATLASES/INFANTMNI_byage/make_INFANTMNI_<age>.sh`, but it's unclear whether this script simply renamed the files or did somethign further 

*Except for 00-02 months,* INFANT_MNI_BRAINMASK has been dilated (DILM, once) from the original NIH age-specific brainmask


**Altered INFANT_MRI atlases used only for brain extraction** \
These files are modified versions of the neonatal files (`INFANT_MNI_T2_1mm.nii.gz` and `INFANT_MNI_T2_1mm_brain.nii.gz`). `temp_nih_T2w_atl_brain.nii.gz` was manually edited to have tighter masking to achieve tighter skull-stripping when used in the pipeline. Importantly, we use these neonatal templates across age groups because this gave us better results than using the age-specific atlases.

The image dimensions are slightly different from the original INFANT_MNI_T2_1mm.nii.gz and brain files, but the purpose for this was undocumented and needs to be explored in the future. Ideally, each age group would have its own refined template, but using the neonate template across all ages currently works well.

If one wishes to use the age-specific templates instead, use the following flag to specify the original head and brain templates: 
`--t2-study-template INFANT_MNI_T2_1mm.nii.gz INFANT_MNI_T2_1mm_brain.nii.gz`

**NOTE:** `temp_nih_T2w_atl_brain.nii.gz` was modified again on 7/10/2020 after fmriprep group noticed it was cutting off superior parietal area. replaced this file with new one in all age ranges. the original file before this specific modification is named temp_nih_T2w_atl_brain_orig.nii.gz under 00-02

**JLF_templates** 
- 00-05 MONTHS JLF_templates contains Albert atlases (Albert_neo_JLF) with segmentations generated and corrected by DCAN

- 08-60 MONTHS JLF_templates contains DCAN_neo_JLF_brains, which were run through JLF using ABCD subs. Formerly known as babyCouncil

**Other** \
The template folders also contain `standard_mesh_atlases` and `91282_Greyordinates` subfolders

Some templates are referenced from &lt;stage>/scripts, using HCPPIPEDIR_Templates, rather than being passed in from the &lt;stage> top-level script. Notes about how these templates are referenced:

* means_RB_all*: used in FreeSurfer/scripts. Don’t move or rename without changing hypernormalize.sh.
* InfMNI_2AdultMNI_Step2.mat: used in fMRISurface/scripts. Don’t move or rename without changing SubcorticalAlign*.sh
Note: INFANT_MNI_T1_2mm_brain_mask_dil.nii.gz does not exist for 00-02. Don’t worry, since the file is referenced by pipelines.py and sent to PreFreeSurfer, but is never actually used.


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).