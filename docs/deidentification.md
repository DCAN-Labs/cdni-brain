## De-Identification of MRI Image Data 

*Please note that de-identification policies can vary between studies and institutions. Be sure to check the specific IRB requirements for your study to ensure your data is thoroughly de-identified according to those standards.*

### Defacing

Defacing is performed in order to mask out the eyes, nose, and mouth from MRI images. These features could hypothetically be used to reconstruct someone's facial structure, which is considered indirect identifiable information. Skull-stripping can also be used for defacing, however this limits what kind of preprocessing can be performed with the data in the event that head images are required.

There are several tools available for defacing, but we recommend using [Pydeface](https://pypi.org/project/pydeface/) (also see the [Neuroimaging Cookbook](https://neuroimaging-cookbook.github.io/recipes/pydeface_recipe/) for more thorough instruction). Pydeface registers a default atlas to the subject head and applies the resulting linear transform to the atlas' accompanying defaced mask to create a subject-specific mask. Pydeface takes one input, the subject head file (see usage for additional options), so simply `pip install pydeface` and run:

```
pydeface subject_head.nii.gz
```

**Specific instructions for infant data**

Defacing infant data is more difficult compared to adults due to contrast inversion (and variable or lower contrast depending on the age and modality) in T1w and T2w images over the first 6 months of age, so it's best to use age-specific atlases in order to get good results. We have generated defaced masks for the 0-2 month old infant NIH MNI T1w and T2w atlases here: `/home/faird/shared/code/internal/utilities/deface/infant_templates` (details for how the defaced masks were generated are included in the README in this directory).    

```
pydeface <T1w subject head> \
--template <INFANT_MNI_T1_1mm.nii.gz> \
--facemask <INFANT_MNI_T1_1mm_defaced_mask.nii.gz>
```

Note that results may vary in infants even when using age-specific atlases. If your T1w and T2w files are registered, one method you can try is to generate a defaced mask for whichever modality resulted in better masking (i.e. more thorough masking of the eyes, nose, and mouth without cutting off any regions of the brain) and apply that mask to the less successful image modality.  

### Skull-stripping with SynthStrip

Instead of defacing, you can also skull-strip anatomical images. We recommend using [SynthStrip](https://surfer.nmr.mgh.harvard.edu/docs/synthstrip/) for this (it even works well for infant data for which skull-stripping is often surprisingly complicated), although remember to assess whether you may need to retain skull-on images for your specific use case. 

Here's an example bash script:

```
#!/bin/bash -l
#SBATCH --job-name=SynthStrip
#SBATCH --time=1:00:00
#SBATCH --mem-per-cpu=40gb
#SBATCH --mail-type=ALL
#SBATCH --mail-user=USER@umn.edu
#SBATCH -A faird

subject_head=sub-01_T1w.nii.gz
outputdir=/path/to/outdir
subject_brain=sub-01_T1w_brain.nii.gz
subject_brainmask=sub-01_T1w_brainmask.nii.gz

module load singularity
singularity=`which singularity`

singularity run \
-B ${subject_head} -B ${outputdir} \
/home/faird/shared/code/external/utilities/synthstrip_1.4.sif \
-i ${subject_head} -o ${subject_brain} -m ${subject_brainmask} 
```
You can also use the `--no-csf` flag for tighter skull-stripping. See the usage for more details.

## Removing identifying information from filenames and metadata
In addition to defacing, you may also need to strip PHI from MRI filenames and metadata. If you have recommendations for additional tools or other helpful information you would like to add to this section, please post an issue to the [CDNI Brain GitHub](https://github.com/DCAN-Labs/cdni-brain/issues)!  

### DICAT (DICOM Anonymization Tool)
For DICOM anonymization, we’ve found [DICAT](https://github.com/aces/DICAT) to be effective and well-documented. Please read the GitHub documentation, but for a quick start:

- Install pydicom: `pip install pydicom` 
- Clone repo locally or use existing clone on MSI available here under `/home/faird/shared/code/external/utilities/DICAT` (may contain additional useful content/notes)
- Prepare csv file with one row per subject/session and 4 columns with the following information:
	- `DCM_DIR`: full path to the DICOM study to deidentify
	- `Pname`  : new patient name to use for deidentification
	- `DOB`    : date to use as a date of birth (can be empty string)
	- `Sex`    : sex of the participant (can be left empty)

> Example CSV file:
```
'/data/John_Doe/','subject_ID_1','1981-01-01','M'
'/data/Lana_Bip/','subject_ID_2','','F'
'/data/Suzy_Bud/','subject_ID_3','1979-10-10',''
```	
- Execute `mass_deidentify.py`, e.g.:
```
python3 mass_deidentify.py -c subject_list_dicoms.csv -v
```
- Once completed, the output folder will contain zip files for both the original and de-identified subject data (e.g. `subject01.zip` and `subject01_deidentified.zip`)

<br>

**ADDITIONAL USAGE TO SPECIFY FIELDS TO ZAP**

By default, DICAT uses the XML file `dicat/data/fields_to_zap.xml` to specify which fields to remove from the DICOMS (including `PatientBirthDate`, `PatientSex`, `PatientID`, etc). However, there is also a more stringent file available if needed, `dicat/data/fields_to_zap_for_open_science.xml`, particularly for open science releases. Whether you would like to customize the XML file or specify the more stringent version, execute as follows:

```
python3 mass_deidentify.py -c subject_list_dicoms.csv -x data/fields_to_zap.xml -v
```


