# Optimizing Job Submissions

Correctly sizing Slurm jobs can help them start sooner, use shared resources more efficiently, and reduce unnecessary impact on your group's fairshare.

For current MSI guidance on estimating CPU, memory, GPU, node, and walltime requirements, see the [MSI Job Sizing and Efficiency documentation](https://userdocs.msi.umn.edu/compute/job_sizing.html).

For CDNI workflows, it is helpful to run a small number of representative subjects before launching a full dataset. These test jobs can be used to estimate the resources required for the larger submission. See our [pipeline-specific resource guidelines](processing.md) when available.

## seff

1. Run a few subjects with the pipeline. You can use the same 3-5 jobs you used for [storage estimates](storage.md) to estimate job specifications. Keep track of the job ID when you submit them.
   
2. After the test jobs complete successfully, review their actual resource usage before scaling up the workflow.

MSI provides job-efficiency information that can be used to compare the resources requested with the resources actually used. CPU, memory, and walltime requests can then be adjusted for future jobs.

For current MSI guidance on interpreting resource usage and right-sizing jobs, see the [MSI Job Sizing and Efficiency documentation](https://userdocs.msi.umn.edu/compute/job_sizing.html).

You can also use:

```bash
sacct -S YYYY-MM-DD -u <x500>
```

## Storage

When you're submitting a job that produces output files, you need to consider where those outputs will be stored (i.e. tier1 vs s3 and on which share). See [the Data Storage page](storage.md) for more information about where and how to store your outputs. Remember, **DO NOT** store ABCC/ABCD data on the `faird` share. 

## Fairshare

MSI uses fairshare as one factor in Slurm job priority. Recent resource usage by a group can reduce the priority of that group's waiting jobs until its relative usage decreases.

For MSI's current explanation of fairshare, billing, resource usage, and scheduling priority, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

You can use:

```bash
sshare -U
```

## Optimizing Your Slurm Queue

The most effective way to improve scheduling is to request resources that closely match what the job actually needs. Oversized requests for memory, CPUs, GPUs, nodes, or walltime can make jobs harder to schedule and increase their impact on group fairshare.

For current MSI recommendations, see the [MSI Job Sizing and Efficiency documentation](https://userdocs.msi.umn.edu/compute/job_sizing.html).

### Partition Choice

Choose a partition whose hardware and resource limits match the requirements of your job.

Partition names, hardware, limits, and availability can change, so see the [MSI Shared Partitions documentation](https://userdocs.msi.umn.edu/compute/shared_partitions.html) for the current available partitions and their resource limits.

When more than one partition is appropriate for a workflow, multiple compatible partitions may be specified. Only include partitions that actually support the resources and requirements of the job.

### Large Job Submissions

For workflows containing many jobs, submit work in a way that respects MSI's current per-user, per-group, and partition limits.

Before launching a large workflow:

- right-size the CPU, memory, walltime, GPU, and temporary-storage requests;
- use the Slurm account associated with the project;
- choose partitions that match the job requirements;
- monitor the first group of jobs for failures or unexpectedly high resource usage;
- use CDNI's [continuous job submission workflow](slurm-params.md#continuous-job-submission) when jobs need to be submitted gradually.

For current job-count limits and partition restrictions, see the [MSI Shared Partitions documentation](https://userdocs.msi.umn.edu/compute/shared_partitions.html).

If jobs remain pending, check the reason shown in `squeue` before changing their configuration. Common causes include unavailable resources, requests exceeding partition limits, user or group limits, fairshare, or MSI maintenance.

For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
