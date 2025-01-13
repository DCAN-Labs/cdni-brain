# Respiratory Filter Parameters
 
This page will explain how to identify the proper band-stop for upper and lower respiration rate settings.

Material presented here is derived from the manuscript [Correction of respiratory artifacts in MRI head motion estimates](https://www.sciencedirect.com/science/article/pii/S1053811919309917) and supporting functions made available in this [repo](https://github.com/DCAN-Labs/movement_regressors_power_plots)

**What are respiratory artifacts?** Several physiological processes distort the data collected during MRI sessions. This includes the subject’s movement related to respiration blurs, classified as "structured noise", i.e. oscillations fluctuating with the subject's breathing rate. For the respiratory artifact, we can use tools from signal processing to minimize their deleterious effect on the BOLD data

**Why should I care?** Respiratory artifacts can lead to an overestimation of head movement. For some groups, the amount of head movement is the preferred QC metric to exclude data from subjects or within-subject segments. Mis-estimation of head movement can lead to flagging data for exclusion, reducing sample size, and increasing the cost of studies since more data would have to be collected to test the intended research question.

**What can I do?** Measures can be do nothing or filter (remove) data excluding components that are more likely to be an artifact. You should, however, always check if the problem exists in your data.

You can use a band-stop filter to remove the artifact induced by the respiratory signal in the estimation of head movement. To do this, you need to specify the cutoff values in **breaths per minute**. Depending on the available data, you can use one of the following approaches to calculate the cutoff values:

1. Use the real values for the range of the respiration range of the subject being processed, *min* and *max* in breaths per minute. This is assuming respiration rate was collected during the MRI. 

2. Read the peaks of frequencies on the Movement Regressor files as described [here](motion-regressor.md) and by visual inspection select an appropriate range to remove signal in the vicinity of the peak value. Finally, convert those frequency values from Hz to breaths per minute by multiplying the number in Hz by 60.

3. If you do not know the real values, pool data from several participants (the more the better, recommended 60 runs at least), use [this tool](https://github.com/DCAN-Labs/movement_regressors_power_plots/blob/master/01_to_run_cat_mov_reg_power.pdf) to look at the concatenated power spectra of the estimates of head movement, then use [this tool](https://github.com/DCAN-Labs/movement_regressors_power_plots/blob/master/03_to_run_get_peaks_from_movement_regressors.pdf) to identify the peaks that are likely related to the respiratory signal, and use the interquartile range of those peaks as cutoff frequency values for the filter. To do this, you can implement a for loop to read the peaks for all the runs. Here is an example:

```
%% Example to read peaks from several runs
%
% In this example the variable LIST has the path to the movement regressor
% files
 
 LIST([1:5 end])
 
ans =
 
  6×1 cell array
 
    {'~/data/anonymized_human/fake_ID_01/fake_visit_1/func/fake_ID_01_fake_visit_1_task-rest_run-1_motion.tsv'}
    {'~/data/anonymized_human/fake_ID_01/fake_visit_1/func/fake_ID_01_fake_visit_1_task-rest_run-2_motion.tsv'}
    {'~/data/anonymized_human/fake_ID_01/fake_visit_1/func/fake_ID_01_fake_visit_1_task-rest_run-3_motion.tsv'}
   {'~/data/anonymized_human/fake_ID_01/fake_visit_1/func/fake_ID_01_fake_visit_1_task-rest_run-4_motion.tsv'}
    {'~/data/anonymized_human/fake_ID_02/fake_visit_1/func/fake_ID_02_fake_visit_1_task-rest_run-1_motion.tsv'}
    {'~/data/anonymized_human/fake_ID_14/fake_visit_1/func/fake_ID_14_fake_visit_1_task-rest_run-4_motion.tsv'}
 
%% Find peaks
% Pre-allocate memory to read peaks
peaks_at=nan(n,6);
 
% Read the peaks using a for loop
for i=1:n
    peaks_at(i,:)=get_peaks_from_movement_regressors(list{i},TR);
end
 
% Calculate the interquartile range
ptiles=[25 75]; % those are the percentile values to be used
cutoff_in_Hz=prctile(peaks_at,ptiles); % read the peaks in Hz in the 6 directions
cutoff_in_bpm=60*prctile(peaks_at,ptiles); % Convert the peaks from Hz to breaths per minute
cutoff_in_bpm_Y_direction=cutoff_in_bpm(:,2); % Extract the peak in the Y direction, i.e., in the second column
%%
```

**REMEMBER TO USE VALUES IN BREATHS PER MINUTE!**


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).