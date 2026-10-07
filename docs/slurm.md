# Slurm Commands

Slurm is MSI's job scheduling and resource management system. Within CDNI, most data processing and analysis workflows that require compute resources are run through Slurm using batch jobs (`sbatch`) or interactive jobs (`srun`).

For current MSI guidance on writing job scripts, submitting jobs, monitoring jobs, and managing resources, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

For interactive sessions using `srun`, see the [MSI Interactive HPC documentation](https://userdocs.msi.umn.edu/compute/interactive_compute.html).

For information about login nodes versus compute nodes, see our [Nodes and Partitions page](partitions.md).

## Job Parameters

Slurm job scripts use `#SBATCH` directives to specify the resources required by a job. Common resource requests include walltime, memory, CPUs, partitions, temporary storage, GPUs, output logs, and email notifications.

For current descriptions and examples of Slurm job parameters, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

For current partition names, walltime limits, memory limits, and available GPU resources, see the [MSI Shared Partitions documentation](https://userdocs.msi.umn.edu/compute/shared_partitions.html).

When creating CDNI jobs, it is especially important to:

- request only the resources your job is expected to need;
- choose an appropriate partition;
- verify the Slurm account being charged;
- verify any email address included in copied job scripts;
- write output and error logs to an appropriate project location.

For CDNI-specific resource optimization guidance, see the [Resource Optimization section](slurm-params.md#resource-optimization).

## Job Status

The commands below are commonly used within CDNI to monitor, inspect, cancel, and modify Slurm jobs.

For MSI's current job-monitoring and management guidance, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

### squeue

`squeue -al --me`: determine specifc jobs for your own account

`squeue -u username`: view all jobs submitted by a given user 

`squeue -A group_name`: check the queue for all groups one is under to determine which to submit under

`squeue -u <username> -h -t pending,running -r | wc -l`: count how many jobs you have in your queue, you can add more statuses if needed

OOD also has a [Jobs Dashboard](https://ondemand.msi.umn.edu/pun/sys/dashboard/activejobs) on their website to see the status of jobs that are running. 

### scancel

`scancel jobID_number`: cancel a submitted job

- Can also be used for job arrays by listing the job ID numbers as a comma separated list

### sacct

`sacct`: display accounting data for all jobs and job steps in the Slurm job accounting log or Slurm database

`sacct -X -j JOBID_ARRAY# -o JobID,NNodes,State,ExitCode,DerivedExitCode,Comment`: check the status of a job even after it has exited, JOBID_ARRAY can also just be JOBID

For current MSI guidance on checking completed jobs and job history, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

### scontrol

`scontrol` can be used to inspect or modify certain properties of pending or running Slurm jobs.

Examples include:

```bash
scontrol update JobId=#### Account=new_group
scontrol update JobId=#### Partition=new_partition
scontrol show JobId=####


`scontrol show JobId=####`: find time information for a job

Some job properties can be modified with `scontrol`, but walltime changes may be restricted depending on the job state and MSI scheduling policy.

For current guidance on walltime and modifying jobs, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

`scontrol release JOBID`: If you see a job in your queue with the status `(launch failed requeued held)` under `NODELIST (REASON)`, you will need to release them to re-enter your queue. Jobs will enter the held state when its launch fails and the scheduler determines that re-queueing will result in the same failed start.

- To loop over multiple jobs with this status, you can use this for loop: `for job in $(squeue -u <x500> -A <group> --state=PD --Format=JobID --noheader);do scontrol release $job; done`



For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
