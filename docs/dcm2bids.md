# DICOM to BIDS Conversion (Dcm2bids)

<div class="admonition attention">
    <p class="first admonition-title">Attention</p>
    <p class="last">
        This document describes usage of Dcm2bids 2.1.x as used in the `dcm2bids` and `dcm2bids_xa30_test` labwide conda environments -- if using the `dcm2bids3` environment (intended for use with the dcm2bids3 NORDIC wrapper), refer to the Dcm2bids 3 documentation page linked below for information on breaking changes introduced in dcm2bids3. We are in the process of updating this page to focus on dcm2bids3 usage, which should be the primary dcm2bids conversion version.
    </p>
</div>

Read: [Dcm2bids 3 documentation page](https://unfmontreal.github.io/Dcm2Bids/latest/)

Watch: [Dcm2bids CDNI tutorial recording](https://drive.google.com/drive/folders/1OgyqFfpqp3qWg4OJzY9ADTwV26j8fJYf) 

`Dcm2bids` is a tool to convert data from DICOM format into proper [brain imaging data structure (BIDS).](https://bids-specification.readthedocs.io/en/stable/)

`dcm2bids` requires a configuration file specifically tailored to the contents of the JSON “sidecar” files that contain metadata for the image files. The config file is a way to uniquely identify each file that needs to be converted.
* Example section: 
    
    ![example dcm2bids](img/dcm2bids-example.png)
    
* Above is an example of a description field, which describes an acquisition, in this case a T2-weighted image. 
    
* The `dataType` field is mandatory for any acquisition. It is a functional group that will define different types of data: func, dwi, fmap, anat, meg, and beh.
    
* The `modalityLabel` is also mandatory. It describes the modality of the acquisition, such as `T1w`, `T2w`, `dwi`, or `bold`.
    
* The `criteria` section is used to match unique identifiers in the sidecar files with values entered in the section. `SeriesDescription` will find any file that contains the specified value. In this example, `SeriesDescription` is looking for any file that contains the characters T2 somewhere in the file name. It is also looking for `EchoTime` fields with a value of 0.1.
    
* The `sidecarChanges` field is optional and it will change or add any information in the sidecar file. In this example, the field `ProtocolName` with the value of T2 is being adjusted in the sidecar.

* To view these fields in the DICOM metadata, you can use `dcmdump` on any .dcm file (on MSI, first `module load dcmtk`)
    
Kimberly Weldon has developed a set of scripts to streamline the conversion process. These scripts will automatically create the config file and run the conversion.
* The needed scripts are all found in `/home/faird/shared/code/internal/utilities/cdniproc/` 
    
* Load the labwide conda environment: `source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh`
    
* Activate the needed conda environment: `conda activate dcm2bids3`
        
    - For multi-echo data, or data acquired on a Siemens scanner with an XA software package (e.g. XA30), use `conda activate dcm2bids3` if using the [Dcm2bids3 NORDIC wrapper](nordic.md) (note you must update your config files to be dcm2bids3 compatible --  see [here](https://unfmontreal.github.io/Dcm2Bids/3.0.2/upgrade/#upgrading-from-2x-to-3x) for a summary of breaking changes between 2.x and 3.x). 
    - If you don't use the dcm2bids3 NORDIC wrapper for multi-echo data, use `conda activate dcm2bids_xa30_test`
    
* `python init.py -d /path/to/dicom/sub-* -p subID -s sesID`

    - **Note:** Don't include a trailing `/` for the dicom directory path and only provide the subject/session labels, without the sub- and ses- prefixes.
    - This will create two files: a summary tsv file and a json config file for dcm2bids, both named `subID-sesID.extension`. 
    - You can optionally include the `--heuristic` flag if you have a heuristic.csv file for your dataset that lists specific SeriesDescriptions that you are trying to convert. 
    - The tsv should label all of the task, rest, fmap, and structural runs. Double check that all runs were correctly labeled and make any necessary updates. If any updates are made, run the same command again with the `--jsononly` flag to update the json.

* `python dcm2bids3.py -f subID-sesID.tsv`

    - Provide the path to the tsv created by the previous command.
    - If the script can't find your dicom directory, you can provide it with `-d /path/to/dicoms/sub-ID`
    - You can provide an output directory with `--outdir /path/to/output/bids/`
    - This script will produce a `bids` directory with the subjects BIDS folder inside.
    - If you're running this multiple times on the same data, you can use the `--noforce` if you don't want to overwrite previous files.

To run your newly converted BIDS data through a processing pipeline, you'll need to fill out the IntendedFor field of any fmap files. This tells the pipeline which fmaps correspond to which BOLD runs in order to use the correct map for distortion correction. If you are planning to run NORDIC on your multi-echo data, you need to do that **before** filling out the IntendedFor fields. There are a few ways to do this. 

1. Use the IntendedFor GUI to manually add the IntendedFor fields to your fmaps. 
    * `source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh`
    * `conda activate intended_for_gui`
    * `python /home/faird/shared/code/internal/utilities/intendedforgui/intended_for_gui.py`
    * Select the BIDS input folder in the browser. This is easier if you were already there in the terminal before running the GUI. 
    * Press `Ctrl` to select the fmaps and all of the corresponding functional runs. 

2. Use Tim Hendrickson's [IntendedFor script](https://github.com/tjhendrickson/BIDS_scripts/blob/master/IntendedFor.py) to automatically determine which fmaps apply to which functional runs.

Please note that the previous lab-standard version of dcm2bids used was 2.1.9. For more information you can read the [Dcm2bids 2.1.9 documentation page.](https://unfmontreal.github.io/Dcm2Bids/2.1.9/) 

For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
