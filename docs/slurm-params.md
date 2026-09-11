# sbatch/srun and Job Submission

Read:

* [Interactive HPC @ MSI](https://userdocs.msi.umn.edu/compute/interactive_compute.html)
* [Slurm Job Submission and Scheduling @ MSI](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html)
* [Shared Partitions @ MSI](https://userdocs.msi.umn.edu/compute/shared_partitions.html)
* [Compute Need to Know @ MSI](https://userdocs.msi.umn.edu/compute/cluster_info.html)

`srun` and `sbatch` are used to run jobs through MSI's Slurm scheduler. `srun` is commonly used for interactive work on a compute node, while `sbatch` submits a script to run as a batch job.

MSI maintains the current documentation for Slurm resources, partitions, job limits, and job submission. The sections below focus on basic usage and CDNI-specific workflows.

## srun

`srun` can be used to start an interactive session on a compute node for testing workflows or running work that should not be performed on a login node.

For current `srun` examples and resource options, see [Interactive HPC @ MSI](https://userdocs.msi.umn.edu/compute/interactive_compute.html).

MSI's [Open OnDemand](https://userdocs.msi.umn.edu/compute/open-ondemand-support.html) is another option for interactive compute and provides browser-based shell access, desktops, Jupyter, MATLAB, and other applications.

## sbatch

`sbatch` submits a job script to Slurm. After the script is successfully submitted, it can wait in the queue and run independently of your terminal session.

To submit a script:

```bash
sbatch scriptname.sh
```

For current job script examples, `#SBATCH` options, resource requests, job arrays, output/error logs, and job monitoring, see [Slurm Job Submission and Scheduling @ MSI](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

For CDNI-specific job management commands, see the [Slurm Commands page](slurm.md). For container and pipeline-specific commands used within batch jobs, see the [Containers page](containers.md) and relevant pipeline documentation.

## Continuous Job Submission

For large workflows or when jobs need to be submitted gradually, CDNI provides a continuous Slurm submitter. The submitter adds jobs to the queue in configurable batches and intervals.

Path:

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

Before submitting a large workflow, check the current [MSI user resource limits](https://userdocs.msi.umn.edu/compute/cluster_info.html#user-resource-limits) and [Shared Partitions](https://userdocs.msi.umn.edu/compute/shared_partitions.html).

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

For questions, suggestions, or to note any errors, [post a GitHub issue](https://github.com/DCAN-Labs/cdni-brain/issues).
