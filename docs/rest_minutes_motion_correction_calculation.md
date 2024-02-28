## Calculating Rest Minutes Pre- and Post-Motion Correction
Assessing motion artifacts in fMRI data is crucial. This guide facilitates calculating rest minutes before and after motion correction, focusing on a predefined FD threshold. Utilizing the MATLAB script calc_min_rest.m streamlines this process. To begin, ensure MATLAB is installed and access to motion.mat files is available.

Locate the script at ***/home/faird/shared/code/internal/analytics/compare_matrices_to_assign_networks/calc_min_rest.m***. Open it in MATLAB to understand its workings, including the hard-coded FD threshold. To facilitate the generation of the conc file, execute the command: 

```
ls -1d <PATH TO DERIVATIVES>/sub-/ses-/func/sub-*task-rest_bold_desc-filtered_motion_mask.mat > motion.conc
```
This command lists motion.mat files and directs the output to motion.conc. Adjust the command if your motion files adhere to a different naming convention.

Next, prepare input files by generating a conc file listing motion.mat files. Execute the script in MATLAB, inputting the conc file when prompted. Upon completion, the script outputs total minutes of rest pre- and post-motion correction. Compare these results to assess data quality.
