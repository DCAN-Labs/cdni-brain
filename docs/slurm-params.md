# sbatch/srun Parameters

MSI uses Slurm to manage compute resources. `srun` is commonly used for interactive compute sessions, while `sbatch` is used to submit non-interactive batch jobs that run through the scheduler.

For current MSI guidance on interactive jobs, see the [MSI Interactive HPC documentation](https://userdocs.msi.umn.edu/compute/interactive_compute.html).

For current MSI guidance on writing and submitting batch jobs, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

## srun

`srun` is used to request interactive compute resources through Slurm. Interactive sessions are useful for testing commands, running software interactively, or performing work that should not be run directly on a login node.

For current `srun` syntax, resource options, X11 usage, and interactive partition guidance, see the [MSI Interactive HPC documentation](https://userdocs.msi.umn.edu/compute/interactive_compute.html).

Within CDNI, make sure the Slurm account you use matches the project or group associated with your work. You can use:

```bash
groupquota
```

## sbatch

`sbatch` is used to submit non-interactive job scripts to the Slurm scheduler. Once submitted, the job is managed by Slurm and can continue running even if you disconnect from MSI.

For current instructions on writing and submitting Slurm job scripts, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

For CDNI-specific Slurm command examples and troubleshooting, see our [Slurm Commands page](slurm.md).

For current job script examples, `#SBATCH` options, resource requests, job arrays, output/error logs, and job monitoring, see [Slurm Job Submission and Scheduling @ MSI](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

A typical CDNI batch script may include:

- a descriptive job name;
- an appropriate partition and Slurm account;
- requested walltime, CPUs, memory, and temporary storage;
- output and error log paths;
- email notifications;
- module loads or container setup;
- the command or pipeline being executed.

For current Slurm parameter syntax and partition limits, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html) and [MSI Shared Partitions documentation](https://userdocs.msi.umn.edu/compute/shared_partitions.html).

When using containers in CDNI workflows, input directories should generally be mounted read-only when appropriate to reduce the risk of accidentally modifying source data.

If you are submitting a job array or pipeline wrapper, output logs may use Slurm placeholders such as:

```text
%A
```
If an sbatch job is running for >3 minutes, there are most likely no errors in the run command, and job doesn't need to be closely monitored before completion.

## Continuous Job Submission
 
For very large CDNI workflows, or when jobs need to be submitted gradually rather than all at once, use the CDNI continuous submitter.

MSI job-count and resource limits can change, so check the [MSI Shared Partitions documentation](https://userdocs.msi.umn.edu/compute/shared_partitions.html) for current per-user and per-group limits before launching a large workflow.

`/projects/standard/faird/shared/code/internal/utilities/SLURM_wrappers/continuous_submitter`

The code and available arguments can be found in the [DCAN-Labs SLURM_wrappers repository](https://github.com/DCAN-Labs/SLURM_wrappers/tree/main/continuous_submitter).

Run the submitter from a persistent session that will remain active while jobs are being submitted.

Example:

```bash
python continuous_slurm_submitter.py \
  --partition msismall \
  --job-name example-job \
  --log-dir /path/to/logs \
  --run_folder /path/to/run_files \
  --n_cpus 8 \
  --time_limit 48:00:00 \
  --total_memory 20 \
  --tmp_storage 100 \
  --array-size 1000 \
  --submission-interval 300 \
  --account_name <account> \
  --emailed_user <x500>@umn.edu
```

`--array-size` controls the number of jobs the submitter attempts to keep in the queue, while `--submission-interval` controls how long it waits between submission attempts.

3. Check the [fairshare](fairshare.md) and confirm which CDNI account is appropriate for the workflow before submitting large numbers of jobs.

For general information about MSI scheduling and fairshare behavior, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

4. If you have a command or script that outputs to a different group than your primary MSI group (i.e. the group with your home directory), you can use `sg` to run as the group that matches the output directory instead. Recommended for abcd-hcp-pipeline, infant-abcd-bids-pipeline, and nhp-abcd-bids-pipeline to avoid permission errors in the FreeSurfer stage of the pipeline.

### Group Permissions

If a workflow writes output to a project group different from your primary MSI group, it may need to run under the group that owns the output directory.

For example:

```bash
sg faird -c "singularity run <...>"
```

The same approach can be used when submitting a batch script:

```bash
sg faird -c "sbatch sbatchscript.sh"
```

This may be useful for workflows such as `abcd-hcp-pipeline`, `infant-abcd-bids-pipeline`, and `nhp-abcd-bids-pipeline` when group permissions would otherwise cause errors.

For Open OnDemand sessions, use MSI's [Open OnDemand Support](https://userdocs.msi.umn.edu/compute/open-ondemand-support.html) for current project group selection guidance.

After submitting a large workflow, check that jobs are not immediately failing because of permissions, job configuration, or data issues.

For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
