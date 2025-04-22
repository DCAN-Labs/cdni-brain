# DICOM to BIDS Conversion (Dcm2bids)

<div class="admonition attention">
    <p class="first admonition-title">Attention</p>
    <p class="last">
        This document describes usage of Dcm2bids 2.1.x as used in the `dcm2bids` and `dcm2bids_xa30_test` labwide conda environments -- if using the `dcm2bids3` environment (intended for use with the dcm2bids3 NORDIC wrapper), refer to the Dcm2bids 3 documentation page linked below for information on breaking changes introduced in dcm2bids3. We are in the process of updating this page to focus on dcm2bids3 usage, which should be the primary dcm2bids conversion version.
    </p>
</div>

Read: [Dcm2bids 2.1.9 documentation page](https://unfmontreal.github.io/Dcm2Bids/2.1.9/)

Read: [Dcm2bids 3 documentation page](https://unfmontreal.github.io/Dcm2Bids/current/)

Watch: [Dcm2bids DCAN tutorial recording](https://drive.google.com/drive/folders/1OgyqFfpqp3qWg4OJzY9ADTwV26j8fJYf) 

`Dcm2bids` is a tool to convert data from DICOM format into proper [brain imaging data structure (BIDS)](https://bids-specification.readthedocs.io/en/stable/) and can be loaded via the labwide Miniconda’s **dcm2bids** environment. (See **(2)** further down in this section.) 


11. `dcm2bids` requires a configuration file specifically tailored to the contents of the JSON “sidecar” files that contain metadata for the image files. The config file is a way to uniquely identify each file that needs to be converted.
    * Example section: 
        
        ![example dcm2bids](img/dcm2bids-example.png)
        
    * Above is an example of a description field, which describes an acquisition, in this case a T2-weighted image. 
        
    * The `dataType` field is mandatory for any acquisition. It is a functional group that will define different types of data: func, dwi, fmap, anat, meg, and beh.
        
    * The `modalityLabel` is also mandatory. It describes the modality of the acquisition, such as `T1w`, `T2w`, `dwi`, or `bold`.
        
    * The `criteria` section is used to match unique identifiers in the sidecar files with values entered in the section. `SeriesDescription` will find any file that contains the specified value. In this example, `SeriesDescription` is looking for any file that contains the characters T2 somewhere in the file name. It is also looking for `EchoTime` fields with a value of 0.1.
        
    * The `sidecarChanges` field is optional and it will change or add any information in the sidecar file. In this example, the field `ProtocolName` with the value of T2 is being adjusted in the sidecar.
    * To view these fields in the DICOM metadata, you can use `dcmdump` on any .dcm file (on MSI, first `module load dcmtk`)
    
12. These series of commands will create the configuration file and convert files from DICOMs to NIfTIs:

    * The needed scripts are all found in `/home/faird/shared/code/internal/utilities/bidsify/dataframe/` 
        
    *  `source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh`
        
    * `conda activate dcm2bids`
            
        - For multi-echo data, or data acquired on a Siemens scanner with an XA software package (e.g. XA30), use `conda activate dcm2bids3` if using the [Dcm2bids3 NORDIC wrapper](nordic.md) (note you must update your config files to be dcm2bids3 compatible --  see [here](https://unfmontreal.github.io/Dcm2Bids/3.0.2/upgrade/#upgrading-from-2x-to-3x) for a summary of breaking changes between 2.x and 3.x). 
        - If you don't use the dcm2bids3 NORDIC wrapper for multi-echo data, use `conda activate dcm2bids_xa30_test`
        
    * `python newInit_df_dev.py -d /path/to/dicom/sub-* -p subID -s sesID --heuristic`

        - **Note:** Don't include a trailing `/` for the dicom directory path and only provide the subject/session labels, without the sub- and ses- prefixes.
        - This will create two files: a summary csv file and a json config file for dcm2bids, both named `subID-sesID.extension`. 
        - The csv should label all of the task, rest, fmap, and structural runs. Double check that all runs were correctly labeled and make any necessary updates. If any updates are made, run the same command again with the `--jsononly` flag to update the json.

    * `python dcm2bids_df.py -f subID-sesID.csv`

        - Provide the path to the csv created by the previous command.
        - If the script can't find your dicom directory, you can provide it with `-d /path/to/dicoms/sub-ID`
        - You can provide an output directory with `--outdir /path/to/output/bids/`
        - This script will produce a `bids` directory with the subjects BIDS folder inside.
        - If you're running this multiple times on the same data, you can use the `--noforce` if you don't want to overwrite previous files.
    
13. For ABCC data, `abcd-dicom2bids`: used for selectively downloading ABCD Study imaging DICOM data QC'ed as good by the ABCD DAIC site (fast track qc), converting it to BIDS standard input data (see [here](https://collection3165.readthedocs.io/en/stable/recommendations/#3-the-bids-quality-control-file)), selecting the best pair of spin echo field maps, and correcting the sidecar JSON files to meet the BIDS Validator specification. `abcd-dicom2bids` still uses `dicom2bids`, but uses a config file that is specific to ABCC. See more information on the [repository](https://github.com/DCAN-Labs/abcd-dicom2bids). For information on how to use the fast track qc, navigate to [this section](fasttrack.md). It is important to note that when running `abcd-dicom2bids` , cache can fill up in your home directory under `/home/{share}/x500/x500/` , so check up on and delete files within this directory when necessary. 
        

    * Example run command for a list of subjects, with good sbatch specs:

    ```
    #!/bin/bash -l
    #SBATCH --job-name=abcd2bids
    #SBATCH --time=12:00:00
    #SBATCH --mem-per-cpu=50gb
    #SBATCH --tmp=80gb
    #SBATCH --output=output_logs/dicom2bids_%A_%a.out
    #SBATCH --error=output_logs/dicom2bids_%A_%a.err
    #SBATCH --mail-type=ALL
    #SBATCH --mail-user=<YOUR_x500>@umn.edu
    #SBATCH -A miran045
    #SBATCH -p amdsmall,small

    source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

    module load singularity

    . "/home/faird/shared/code/external/envs/miniconda3/mini3/etc/profile.d/conda.sh"
    export PATH="/home/faird/shared/code/external/envs/miniconda3/mini3/bin:$PATH"
    export PATH=$PATH:/home/feczk001/shared/code/external/utilities/dcmtk-3.6.5/dcmtk-3.6.5-build/bin
    export PATH=$PATH:/home/feczk001/shared/code/external/utilities/jq-1.5

    python3 /home/rando149/shared/code/internal/utilities/abcd-dicom2bids/abcd2bids.py /common/software/install/migrated/fsl/6.0.4/ /home/feczk001/shared/code/external/utilities/MATLAB_MCR/v91/ -q /home/rando149/shared/code/internal/utilities/abcd-dicom2bids/spreadsheets/fastqc20230912/abcd_fastqc01.txt -l /path/to/subject_list/subjects.txt -o /path/to/output_dir/ -p 1219957 --download=/path/to/raw/data/downloaded --downloadcmd /home/faird/shared/code/external/envs/miniconda3/mini3/envs/abcd-dicom2bids/bin/downloadcmd --singularity /home/rando149/shared/code/internal/utilities/abcd-dicom2bids_nda/validator_latest.sif 
    ```
        
    - In the run command, `-q` is the path to the most recent quality control (QC) spreadsheet file downloaded from the NDA,`-l` is the list of subjects being converted, `-o` is where the outputs are being stored, `-p` is the package ID number of relevant NDA data package, `--downloadcmd` is the path where the downloadcmd has been installed.
        
    - For more information on arguments, see the ReadMe section on Optional Arguments [here](https://github.com/DCAN-Labs/abcd-dicom2bids).
        
    - For running multiple subjects, it is advised to use a [wrapper](wrappers.md) to submit one subject/session at a time. There is a wrapper for tier 1 here: `/home/faird/shared/code/internal/utilities/SLURM_wrappers/ABCD-related/ABCD-BIDS_disk` and a wrapper for tier 2 here: `/home/faird/shared/code/internal/utilities/SLURM_wrappers/ABCD-related/ABCD-BIDS_ses_s3`
            
        * Note: if you do not have access to the `faird` share, please fill out [this contact form](https://innovation.umn.edu/developmental-cognition-and-neuroimaging-lab/contact-us/) to request access.


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).
