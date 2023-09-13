# NORDIC denoising and Dcm2bids3 NORDIC wrapper

NORDIC (**NO**ise **R**eduction with **DI**stribution **C**orrected) is a denoising method that can be applied to functional data. For our use cases, each run to be denoised should have separate NIfTI timeseries for each of the magnitude and phase data.

The recommended method to apply NORDIC denoising is using the **Dcm2bids3 NORDIC wrapper** with the **dcm2bids3** conda environment. To use:

- Make **two** dcm2bids config files for your data: the first for converting everything **except** the phase timeseries, and the second for converting **only** the phase timeseries.
  - Note that Dcm2bids 2 config files are **not** compatible with Dcm2bids 3! (The **dcm2bids** and **dcm2bids_xa30_test** environments use Dcm2bids 2)
  - See documentation for how to "upgrade" old-version config files (https://unfmontreal.github.io/Dcm2Bids/3.0.2/upgrade/#description-keys)
  - Also see documentation of the new "post-op" command feature (https://unfmontreal.github.io/Dcm2Bids/3.0.2/how-to/use-advanced-commands/#post_op), which we use to execute the NORDIC wrapper via sbatch job submission 
- When specifying your `custom_entities` for magnitude and phase timeseries, use the BIDS-compliant `part-mag` and `part-phase` 
- In the second config file, include a "post_op" command that calls the NORDIC wrapper script like this (do **not** replace "src_file" and "dst_file" with actual file paths -- Dcm2bids will do it automatically -- but **do** replace the "3" with the actual number of "noise volumes" at the end of each run)
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
- Load the **dcm2bids3** conda environment and run dcm2bids on your dataset with the first config file (everything but phase), **then** run dcm2bids again with the second config file (to convert phase data and run the NORDIC wrapper via post-op command)

---

**Alternatively**, there is the now-deprecated BIDS NORDIC wrapper at `/home/faird/shared/code/internal/utilities/bids_nordic_wrapper`, which should be run using the **dcm2bids_xa30_test** conda environment. It takes a BIDS func directory (and number of noise volumes per scan) as input, runs the NORDIC script on the magnitude-phase file pairs, and includes a cleanup script (to be run separately) to remove intermediate files created in the process. The wrapper expects a specific naming convention for the magnitude and phase timeseries; described in (1) below. From the README:


_Usage_


_1) Convert source data to BIDS with Dcm2bids. BIDS task names for the converted files are expected to be as follows:_


_"task-restSE" and "task-restSEph", respectively for single-echo magnitude and phase BOLD timeseries_


_"task-restME" and "task-restMEph" for multi-echo_


_(An example dcm2bids config file which outputs the expected task names is in "dcm2bids_conf_examples")_

---

NORDIC can also be run using standalone Matlab script at `/home/faird/shared/code/external/utilities/NORDIC/NIFTI_NORDIC.m`
 


_2) Run bids_nordic_wrapper.sh (arg1= path to func dir, arg2= number of noise volumes at end of each run)_


_3) [Optional] Run nordic_cleanup.sh to remove logs and other files not used in further processing_
