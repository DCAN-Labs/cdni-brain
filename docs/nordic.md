# NORDIC denoising and Dcm2bids3 NORDIC wrapper

NORDIC (**NO**ise **R**eduction with **DI**stribution **C**orrected) is a denoising method that can be applied to functional data. For our use cases, each run to be denoised should have separate NIfTI timeseries for each of the magnitude and phase data.

## dcm2bids3

The recommended method to apply NORDIC denoising is using the dcm2bids3 NORDIC wrapper with the `dcm2bids3` conda environment. To use:

- Make **two** dcm2bids config files for your data: the first for converting everything **except** the phase timeseries, and the second for converting **only** the phase timeseries.

    - Also see [documentation of the new "post-op" command feature](https://unfmontreal.github.io/Dcm2Bids/3.0.2/how-to/use-advanced-commands/#post_op), which we use to execute the NORDIC wrapper via sbatch job submission.

- When specifying your `custom_entities` for magnitude and phase timeseries, use the BIDS-compliant `part-mag` and `part-phase` 
- In the second config file, include a "post_op" command that calls the NORDIC wrapper script like this:
    - Do **not** replace "src_file" and "dst_file" with actual file paths, dcm2bids will do it automatically 
    - **Do** replace the "3" with the actual number of "noise volumes" at the end of each run. 
    - Use `--wait` so that the sbatch command returns when the submitted job completes, instead of when the job is submitted.

```
    "post_op": [
        {
            "cmd": "sbatch --wait /home/faird/shared/code/internal/utilities/Dcm2bids3_NORDIC_wrapper/nordicsbatch.sh src_file dst_file 3",
            "datatype": "func",
            "suffix": [
                "bold"
            ]
        }
    ]
```
- Load the `dcm2bids3` conda environment and run dcm2bids on your dataset with the first config file (everything but phase), **then** run dcm2bids again with the second config file (to convert phase data and run the NORDIC wrapper via post-op command). **Use the same options for input / output paths, particpant and session ids on both runs -- the only difference is the config file used**.

Notes:
- Dcm2bids 2 config files are **not** compatible with Dcm2bids 3! (The `dcm2bids` and `dcm2bids_xa30_test` environments use Dcm2bids 2).
  -  See [documentation for how to "upgrade" old-version config files](https://unfmontreal.github.io/Dcm2Bids/3.0.2/upgrade/#description-keys)
  - A script to convert Dcm2bids 2 config files to Dcm2bids 3 is at `/home/faird/shared/code/internal/utilities/Dcm2bids3_NORDIC_wrapper/` 
    - Usage: `python3 convert_dcmbids2_config.py /path/to/dcm2bids2_config.json paht/to/dcm2bids3_config.json`
    - This script does **not** split the config file into two parts as expected by the wrapper, nor does it add a post-op command to run NORDIC.

## dcm2bids_xa30

**Alternatively**, there is the now-deprecated BIDS NORDIC wrapper at `/home/faird/shared/code/internal/utilities/bids_nordic_wrapper`, which should be run using the **dcm2bids_xa30_test** conda environment. It takes a BIDS func directory (and number of noise volumes per scan) as input, runs the NORDIC script on the magnitude-phase file pairs, and includes a cleanup script (to be run separately) to remove intermediate files created in the process. The wrapper expects a specific naming convention for the magnitude and phase timeseries; described below. From the README:

_Usage_

1. Convert source data to BIDS with dcm2bids. BIDS task names for the converted files are expected to be as follows:

    - "task-restSE" and "task-restSEph", respectively for single-echo magnitude and phase BOLD timeseries
    - "task-restME" and "task-restMEph" for multi-echo
    - An example dcm2bids config file which outputs the expected task names is in "dcm2bids_conf_examples

2. Run `bids_nordic_wrapper.sh /path/to/func/dir <# noise vols>` (arg2= number of noise volumes at end of each run)

3. [Optional] Run nordic_cleanup.sh to remove logs and other files not used in further processing  (arg1= path to func dir)

NORDIC can also be run using standalone Matlab script at `/home/faird/shared/code/external/utilities/NORDIC/NIFTI_NORDIC.m`
 
If you used the steps described [here](dcm2bids.md) you can run this script for NORDIC `/home/faird/shared/code/internal/utilities/bidsify/dataframe/runNordic_df.py -f subID-sesID.csv`

## Known issues

**Datatype issue**

The `NIFTI_NORDIC.m` MATLAB script, which is used by both the above methods, will fail when the input data is a 32-bit datatype (e.g. INT32, FLOAT32). No error message is given: "Completing NORDIC..." will be written to the terminal / log file, but the output .nii file will not be made. 

Typically, for CMRR's NORDIC-compatible sequences, the dcm2bids-converted NIfTI files should be 16-bit, **but** some functions (e.g. FSL's `fslroi`) can convert to 32-bit which results in the NORDIC error.

One workaround is to use fslmaths with an identity operation and `-odt` to change the datatype without otherwise changing the data :`fslmaths ${input_file} -mul 1 ${output_file} -odt short` (the `short` datatype is 16-bit).  


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).