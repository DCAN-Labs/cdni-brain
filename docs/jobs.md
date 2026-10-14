# Job Submissions

24. After submitting jobs, check the first several jobs for immediate failures or unexpected behavior before allowing a large workflow to continue. Review Slurm output/error logs and job status as needed.

For current MSI guidance on monitoring jobs, interpreting job states, and reviewing completed jobs, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

25. For large CDNI workflows that approach MSI's current scheduler limits, use the continuous submitter to submit jobs gradually.

For current MSI job-count and scheduler limits, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html) and [MSI Shared Partitions documentation](https://userdocs.msi.umn.edu/compute/shared_partitions.html).

    * * The CDNI continuous submitter is available at:
  ```text
  /projects/standard/faird/shared/code/internal/utilities/slurm_pipeline_wrappers/continuous_slurm_array_submitter
   ```

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

26. 26. Use the MSI account associated with the project or data being processed. See our [fairshare page](fairshare.md) for CDNI-specific context, and the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html) for MSI's current explanation of fairshare and job priority.
        
27. If a command or script writes output to a project owned by a different MSI group than your current group context, you can use `sg` to run the command under the group associated with the output location. This is recommended for workflows such as `abcd-hcp-pipeline`, `infant-abcd-bids-pipeline`, and `nhp-abcd-bids-pipeline` when needed to avoid permission errors.

Example for when your output directory is on faird:
`sg faird -c "singularity run <...>"`

This includes batch scripts. (e.g. `sg faird -c "sbatch sbatchscript.sh"`)

More info on sg here: https://linux.die.net/man/1/sg

28. After starting a large submission, verify that jobs are not failing immediately. Common causes include permission problems, incorrect job configuration, invalid paths, and data issues.

If jobs remain pending rather than failing, check the reason reported by `squeue`. MSI documents common pending reasons and job states in the [Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).
