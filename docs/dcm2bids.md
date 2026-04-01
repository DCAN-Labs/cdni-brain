# DICOM to BIDS Conversion (Dcm2bids)

<div class="admonition attention">
    <p class="first admonition-title">Attention</p>
    <p class="last">
        Please note that this page has been mostly updated to describe the conversion process using dcm2bids3. It is highly recommended to update your workflow to use this dcm2bids version. 
    </p>
</div>

Read: [Dcm2bids 3 documentation page](https://unfmontreal.github.io/Dcm2Bids/latest/)

Watch: [Dcm2bids CDNI tutorial recording](https://drive.google.com/drive/folders/1OgyqFfpqp3qWg4OJzY9ADTwV26j8fJYf) 

`Dcm2bids` is a tool to convert data from DICOM format into proper [Brain Imaging Data Structure (BIDS).](https://bids-specification.readthedocs.io/en/stable/)

`dcm2bids` requires a configuration file specifically tailored to the contents of the JSON “sidecar” files that contain metadata for the image files. The config file is a way to uniquely identify each file that needs to be converted.

* Example section: 

```
{
    "descriptions": [
        {
            "datatype": "anat",
            "suffix": "T1w",
            "custom_entities": "run-01",
            "criteria": {
                "SidecarFilename": "007_*",
                "SeriesDescription": "ABCD_T1w_MPR"
            }
        }
    ]
}
```
    
* Above is an example of a description field, which describes an acquisition, in this case a T1-weighted image. 
    
* The `datatype` field is mandatory for any acquisition. It is a functional group that will define different types of data: func, dwi, fmap, anat, meg, and beh.
    
* The `suffix` is also mandatory. It describes the modality of the acquisition, such as `T1w`, `T2w`, `dwi`, `epi`, or `bold`.

* The `custom_entities` section allows you to add additional information about the run, such as a run number, acquisition label (i.e. `acq-dwi`), or direction (i.e. `dir-PA`)
    
* The `criteria` section is used to match unique identifiers in the sidecar files with values entered in the section. `SeriesDescription` will find any file that contains the specified value. In this example, `SeriesDescription` is looking for any file that contains ABCD_T1w_MPR somewhere in the file name. It is also looking for `SidecarFilename` fields that starts with 007_, which is the series number in the DICOM folders.

* To view these fields in the DICOM metadata, you can use `dcmdump` on any .dcm file (on MSI, first `module load dcmtk`)
    
Kimberly Weldon has developed a set of scripts to streamline the conversion process. These scripts will automatically create the config file and run the conversion.

<div class="admonition attention">
    <p class="first admonition-title">Attention</p>
    <p class="last">
        Please note that this new conversion workflow is still being tested to ensure generalizability. If you run into any unexpected outcomes, please talk with Kim.
    </p>
</div>

* The needed scripts are all found in `/projects/standard/faird/shared/code/internal/utilities/cdniproc_v2.0/` 
    
* Load the labwide conda environment: `source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh`
    
* Activate the needed conda environment: `conda activate py11`
    
* `python tools/convert_helper.py -d /path/to/dicoms/sub-ID -p subID -s sesID -b /path/to/bids`

    - **Note:** Don't include a trailing `/` for the dicom directory path and only provide the subject/session labels, without the sub- and ses- prefixes.
    - This will run dcm2bids_helper to convert all of the DICOMS to NIFTI files and place the outputs in the bids/tmp_dcm2bids/sub-ID folder so that the next script can create the tsv file. 

* `python nii_init_gpt5.py /path/to/bids/tmp_dcm2bids/sub-ID -p subID -s sesID -o /path/to/output.tsv`

    - Run `module load fsl` first!
    - Provide the path to the output of the convert_helper script.
    - You can either provide the subject and session IDs (with or without the sub- and ses- labels), which will allow the script to create a tsv with the format subID_sesID.tsv in the directory from which you ran the script, or you can provide the path to where and what you want the tsv file to be named.
    - This script will iterate through the converted NIFTI files to create a summary tsv that will label the runs of interest. 

* `python tsv_to_json.py /path/to/summary.tsv`

    - Provide the path to the tsv file created from the nii_init script.
    - This code will convert the summary tsv file into a dcm2bids config file.

* `dcm2bids -d /path/to/dicoms/sub-ID -p subID -s sesID -c subID_sesID.json -o /path/to/bids`

    - This command will quickly rename the converted files into BIDS format and place them in the bids folder. 

To run your newly converted BIDS data through a processing pipeline, you'll need to fill out the IntendedFor field of any fmap files. This tells the pipeline which fmaps correspond to which BOLD runs in order to use the correct map for distortion correction. If you are planning to run NORDIC on your multi-echo data, you need to do that **before** filling out the IntendedFor fields. There are a few ways to do this. 

1. Use the IntendedFor GUI to manually add the IntendedFor fields to your fmaps. 
    * `source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh`
    * `conda activate intended_for_gui`
    * `python /projects/standard/faird/shared/code/internal/utilities/intendedforgui/intended_for_gui.py`
    * Select the BIDS input folder in the browser. This is easier if you were already there in the terminal before running the GUI. 
    * Press `Ctrl` to select the fmaps and all of the corresponding functional runs. 

2. Use Tim Hendrickson's [IntendedFor script](https://github.com/tjhendrickson/BIDS_scripts/blob/master/IntendedFor.py) to automatically determine which fmaps apply to which functional runs.

Please note that the previous lab-standard version of dcm2bids used was 2.1.9. For more information you can read the [Dcm2bids 2.1.9 documentation page.](https://unfmontreal.github.io/Dcm2Bids/2.1.9/) 

For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
