# Slurm Commands

Read:

* [Slurm @ MSI documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html)
* [Slurm official site](https://slurm.schedmd.com/documentation.html)
* [Slurm commands cheat sheet](https://slurm.schedmd.com/pdfs/summary.pdf)

**Slurm** is MSI's job scheduling system. CDNI uses Slurm for most data processing and analysis jobs.

For general information about writing job scripts, requesting resources, submitting jobs, GPU jobs, job arrays, monitoring jobs, canceling jobs, and Fairshare, refer to the [MSI Slurm documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

This page focuses on commands and workflows that are particularly useful for CDNI users.

## Interactive Jobs

Slurm interactive jobs (`srun`) provide access to compute resources directly from the terminal, rather than running a batch job in the background with `sbatch`.

An interactive job places you on a compute node and can be useful for testing or running commands that require more resources or longer runtime than is appropriate on a login node.

Read more about login vs. compute nodes on the [partition page](partitions.md#nodes).

## CDNI Slurm Accounts

CDNI users may have access to multiple Slurm group accounts.

Check jobs associated with a particular group:

` squeue -A <group> `

Change the account for an already submitted job:

` scontrol update JobId=<job_id> Account=<group> `

This can be useful when working across multiple PI or group accounts.

For general information about Slurm scheduling and Fairshare, see the [MSI Slurm documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

## Useful Queue Commands

Count your pending and running jobs:

` squeue -u <x500> -h -t pending,running -r | wc -l `

You can also view active jobs using the [MSI Open OnDemand Jobs Dashboard](https://ondemand.msi.umn.edu/pun/sys/dashboard/activejobs).

For general `squeue`, `sacct`, and `scancel` usage, refer to the [MSI Slurm documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

## Changing a Submitted Job

Change the account:

` scontrol update JobId=<job_id> Account=<group> `

Change the partition:

` scontrol update JobId=<job_id> Partition=<partition> `

Refer to MSI documentation for current partition and resource information.

## Releasing Held Jobs

If a job appears in the queue with the status `(launch failed requeued held)`, release it so it can re-enter the queue:

` scontrol release <job_id> `

To release multiple pending jobs for an account:

` for job in $(squeue -u <x500> -A <group> --state=PD --Format=JobID --noheader); do scontrol release $job; done `

For questions, suggestions, or to note any errors, [post a GitHub issue](https://github.com/DCAN-Labs/cdni-brain/issues).
