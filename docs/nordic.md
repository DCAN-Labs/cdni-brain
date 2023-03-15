# NORDIC denoising and BIDS NORDIC wrapper

NORDIC (**NO**ise **R**eduction with **DI**stribution **C**orrected) is a denoising method that can be applied to functional data. It requires that both magnitude and phase data NIfTIs exist for each run to be denoised.


A Matlab script to apply NORDIC is located at `/home/feczk001/shared/code/external/utilities/NORDIC/20211206/NIFTI_NORDIC.m`


The minimum required inputs to the script are the magnitude and phase files, plus a filename for the denoised output. 


Additionally, the number of noise volumes at the end of the run should be specified with the ARG.noise_volume_last argument. Noise volumes, if present, should be removed from the output with `fslroi` (module load fsl; fslroi &lt;input> &lt;output> &lt;tmin> &lt;tsize>). “tmin” will be 0, and “tsize” should be the number of volumes in the timeseries **excluding** noise volumes at the end. 


**Alternatively**, there is a BIDS NORDIC wrapper at `/home/faird/shared/code/internal/utilities/bids_nordic_wrapper` which takes a BIDS func directory (and number of noise volumes per scan) as input, runs the NORDIC script on the magnitude-phase file pairs, and includes a cleanup script to remove intermediate files created in the process. The wrapper expects a specific naming convention for the magnitude and phase timeseries; described in (1) below. From the README:


_Usage_


_1) Convert source data to BIDS with Dcm2bids. BIDS task names for the converted files are expected to be as follows:_


_"task-restSE" and "task-restSEph", respectively for single-echo magnitude and phase BOLD timeseries_


_"task-restME" and "task-restMEph" for multi-echo_


_(An example dcm2bids config file which outputs the expected task names is in "dcm2bids_conf_examples")_


_2) Run bids_nordic_wrapper.sh (arg1= path to func dir, arg2= number of noise volumes at end of each run)_


_3) [Optional] Run nordic_cleanup.sh to remove logs and other files not used in further processing_