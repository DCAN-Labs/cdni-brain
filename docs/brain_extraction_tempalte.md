# Templates for Brain Extraction

For brain extraction we actually use the **temp_nih_T2w_atl.nii.gz** and **temp_nih_T2w_atl_brain.nii.gz** files, which are modified versions of the neonatal **INFANT_MNI_T2_1mm.nii.gz** and **INFANT_MNI_T2_1mm_brain.nii.gz** files that were manually edited to have tighter masking.


Importantly, we use these neonatal templates across age groups because this gave us better results than using the age-specific atlases.

If one wishes to use the age-specific templates instead, use the following flag to specify the original head and brain templates: 
`--t2-study-template INFANT_MNI_T2_1mm.nii.gz INFANT_MNI_T2_1mm_brain.nii.gz`

Templates README file: 


FROM 02-60 MONTHS TEMPLATES INCLUDE:

-NIH age-specific atlases in MNI space called INFANT_MNI. These were created using /home/groups/brainmri/infant/NIH_ATLASES/INFANTMNI_byage/make_INFANTMNI_[eachage.sh](http://eachage.sh/) after downloading them from [http://nist.mni.mcgill.ca/?p=1005](http://nist.mni.mcgill.ca/?p=1005). Except for 00-02 months, INFANT_MNI_BRAINMASK has been dilated (DILM, once) from the original NIH age-specific brainmask

- 91282_Greyordinates/
 
- temp_nih_T2w_atl.nii.gz: this is the neonate (00-02 month old) INFANT_MNI_T2_1mm.nii.gz template with a mask (temp_nih_T2w_atl_brain.nii.gz) that has been manually modified for tighter skull-stripping. These files are used only for brain extraction. The image dimensions are slightly different from the original INFANT_MNI_T2_1mm.nii.gz and brain files, but the purpose for this was undocumented and needs to be explored in the future. Ideally, each age group would have its own refined template, but using the neonate template across all ages currently works well.

- temp_nih_T2w_atl_brain.nii.gz: manually modified neonate (00-02 month old) INFANT_MNI_T2_1mm_brain.nii.gz (see point above)

NOTE: modified this file again on 7/10/2020 after fmriprep group noticed it was cutting off superior parietal area. replaced this file with new one in all age ranges. the original file before this specific modification is named temp_nih_T2w_atl_brain_orig.nii.gz under 00-02

- standard_mesh_atlases/

- JLF_templates: a softlink to the appropriate set of templates in JLF_templates at this level.

More on JLF_templates directory:

- 00-05 MONTHS JLF_templates contains Albert atlases (Albert_neo_JLF) with segmentations generated and corrected by DCAN

- 08-60 MONTHS JLF_templates contains DCAN_neo_JLF_brains, which were run through JLF using ABCD subs. Formerly known as babyCouncil

Some templates are referenced from &lt;stage>/scripts, using HCPPIPEDIR_Templates, rather than being passed in from the &lt;stage> top-level script. Notes about how these templates are referenced:


* means_RB_all*: used in FreeSurfer/scripts. Don’t move or rename without changing hypernormalize.sh.
* InfMNI_2AdultMNI_Step2.mat: used in fMRISurface/scripts. Don’t move or rename without changing SubcorticalAlign*.sh
Note: INFANT_MNI_T1_2mm_brain_mask_dil.nii.gz does not exist for 00-02. Don’t worry, since the file is referenced by pipelines.py and sent to PreFreeSurfer, but is never actually used.
PS: note that the **INFANT_MNI_T2_1mm.nii.gz** and **temp_nih_T2w_atl.nii.gz** files, while they hypothetically should be the same, have slightly different image dimensions. Unfortunately the purpose for this was not documented when these files were originally created and we still need to figure that out why this was done.
