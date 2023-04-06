# Processing Status 

Produce status html on the output data to see how many processing jobs succeeded and failed (only available for abcd-hcp and infant-abcd-bids pipelines).

1. If your dataset is more than 10 subjects, start an interactive session first: `srun -N 1 --cpus-per-task=1  --mem-per-cpu=5gb -A feczk001 -t 6:00:00 -p interactive --x11 --pty bash`

2. Load lab-wide miniconda3 environment and activate the abcd-hcp-pipeline_audit: `source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh`

    `conda activate abcd-hcp-pipeline_audit`

3. Navigate to the correct directory: cd `/home/faird/shared/code/internal/utilities/abcd-hcp-pipeline_audit`

4. To run on data that's on the s3, s3 keys are needed (run s3info in the command line, then see 1. below). To run on tier 1, see 2.

    1. S3: 

            python3 run.py --s3_access_key CENSORED --s3_secret_key CENSORED --report_output_dir path/to/the/directory/where/you/want/the/outputs/ s3://bucket_name/ s3://bucket_name/path/to/tier2/outputs group

    2. Tier 1: 

            python3 run.py --report_output_dir path/to/the/directory/where/you/want/the/outputs/ /path/to/tier1/input/ /path/to/tier1/output/ group

5. Will produce a s3_status_report.html and .csv. These files give information on the results of each processing step for each subject/session pair. A tip for determining subjects’ processing status counts is to download the .csv into excel, create a pivot table of the data, and provide session ID in the values section and the name of the last column in the rows section. 