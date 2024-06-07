# Precision Mapping Wrapper
Read:_ [Rapid Precision Functional Mapping of Individuals Using Multi-Echo fMRI ](https://pubmed.ncbi.nlm.nih.gov/33357444/)_(Lynch et al. 2020)

**Parcellated functional connectivity (FC)**


**_Time x FC reliability curve._** The script at  &lt;location> generates a reliability curve from parcellated BOLD timeseries files (`.ptseries.nii`) in an XCP-D derivatives directory. It is a Slurm sbatch script which requires a list of positional arguments to run. 


The general form of the run command is 


`sbatch /home/faird/shared/code/internal/analytics/precision_mapping_for_workshop/code_sorted/main_run_script_reliability_curve.sh <DERIVATIVESDIR> <OUTDIR> <SUB> <SES> <TASK> <FD> <NUM> <MIN>`   , where: 


    DERIVATIVESDIR = Path to an XCP-D BIDS derivatives directory


    OUTDIR = Directory to write output to  


    SUB = BIDS subject ID (without the “sub-”)


    SES = BIDS session ID (without the “ses-”)


    TASK = BIDS task name (without the “task-”)


    FD = Framewise displacement (FD) threshold, in mm. Is **not** required to be the same FD threshold used with XCP-D. Recommended: between 0.1 and 0.2.


    NUM = Number of permutations to run. (In each permutation, a block of consecutive minutes is sampled   The final reliability values in the curve will be an average over all iterations. Amount of time needed to run increases with the number of iterations.  Recommended: 10 or more.


    MIN = Split-half length limit in minutes, i.e. how far the x-axis of the curve plot extends. Must be an integer. Must be no more than half the total time of TASK specified above.


_Notes:_

* XCP-D version must be >= 0.3.0
* XCP-D must be run with both `--cifti`   (CIFTI output) and   `--m` (concatenate the runs within each BIDS task)
* Currently only Gordon333 parcellated timeseries are supported  (*_space-fsLR_atlas-Gordon_den-91k_timeseries.ptseries.nii).

**(TBA - FC reliability matrix, dense FC curve, dense FC matrix)**


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).