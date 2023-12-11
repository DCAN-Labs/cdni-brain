# Job Submissions

24. If an sbatch job is running for >3 minutes, there are most likely no errors in the run command, and job doesn't not need to be closely monitored before completion 

25. If a submission is above 2000 jobs (slurm’s max in-queue number of jobs), use the continuous submitter.

    * Path to continuous submitter: `/home/faird/shared/code/internal/utilities/slurm_pipeline_wrappers/continuous_slurm_array_submitter`

    * Below is an example of what the submitter script will look like:

```
#!/bin/bash -l
#SBATCH -J continuous_slurm_submitter
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=3
#SBATCH --cpus-per-task=1
#SBATCH --mem=6gb
#SBATCH -t 168:00:00
#SBATCH --mail-type=ALL
#SBATCH --mail-user=<YOURx500>@umn.edu
#SBATCH -p max
#SBATCH -o continuous_slurm_submitter_%A.out
#SBATCH -e continuous_slurm_submitter_%A.err
#SBATCH -A ACCOUNT

source /home/umii/hendr522/SW/miniconda3/etc/profile.d/conda.sh

python continuous_slurm_submitter.py --partition small,amdsmall --job-name abcd-hcp-pipeline_full --log-dir cont_slurm_submitter_logs --run_folder /path/to/project/folder/abcd-hcp-pipeline_scripts_with_s3_routines_with_ses/run_files.abcd-hcp-pipeline_full/ --n_cpus 8 --time_limit 48:00:00 --total_memory 20 --tmp_storage 100 --array-size 1000 --submission-interval 300 --account-name feczk001
```

26. Check the [fairshare](fairshare.md) to know which account to use for processing.
27. If you have a command or script that outputs to a different group than your primary MSI group (i.e. the group with your home directory), you can use `sg` to run as the group that matches the output directory instead. Recommended for abcd-hcp-pipeline, infant-abcd-bids-pipeline, and nhp-abcd-bids-pipeline to avoid permission errors in the FreeSurfer stage of the pipeline.

Example for when your output directory is on faird:
`sg faird -c "singularity run <...>"`

This includes batch scripts. (e.g. `sg faird -c "sbatch sbatchscript.sh"`)

More info on sg here: https://linux.die.net/man/1/sg

28. Make sure a ton of jobs aren’t failing right away. Permissions errors, job set up issues, and data issues are common causes.
