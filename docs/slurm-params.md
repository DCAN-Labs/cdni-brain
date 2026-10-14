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

1. Results are written out as your script specifies. `stdout` and `stderr` will be outputted if the `-o` and `-e` flags are specified, and you can submit other commands right away.
2. An sbatch job is handled by Slurm; you can disconnect (not run interactively), kill your terminal, etc. with no consequence
3. To run an sbatch, use this command: `sbatch scriptname.sh`

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

* Path to continuous submitter: `/projects/standard/faird/shared/code/internal/utilities/SLURM_wrappers/continuous_submitter`

* The code can be found [on GitHub.](https://github.com/DCAN-Labs/SLURM_wrappers/tree/main/continuous_submitter)

* This command needs to be run on a persistent desktop, or at least a desktop with as many hours as it will take to submit all the jobs based on how far apart the submission interval is. 

* Below is an example of what the submitter command will look like:

```
python continuous_slurm_submitter.py --partition small,amdsmall --job-name abcd-hcp-pipeline_full 
--log-dir /path/to/cont_slurm_submitter_logs --run_folder /path/to/project/folder/run_files.abcd-hcp-pipeline_full/ 
--n_cpus 8 --time_limit 48:00:00 --total_memory 20 --tmp_storage 100 --array-size 1000 
--submission-interval 300 --account-name feczk001 --mail_user x500@umn.edu
```

The `array-size` is how many jobs you are submitting at once. The `submission-interval` is the amount of time in minutes to wait until submitting another array of jobs. 

3. Check the [fairshare](fairshare.md) and confirm which CDNI account is appropriate for the workflow before submitting large numbers of jobs.

For general information about MSI scheduling and fairshare behavior, see the [MSI Slurm Job Submission and Scheduling documentation](https://userdocs.msi.umn.edu/compute/slurm_job_submission.html).

4. If you have a command or script that outputs to a different group than your primary MSI group (i.e. the group with your home directory), you can use `sg` to run as the group that matches the output directory instead. Recommended for abcd-hcp-pipeline, infant-abcd-bids-pipeline, and nhp-abcd-bids-pipeline to avoid permission errors in the FreeSurfer stage of the pipeline.

    * Example for when your output directory is on faird: `sg faird -c "singularity run <...>"`

    * This includes batch scripts. (e.g. `sg faird -c "sbatch sbatchscript.sh"`)

    * [More info on sg](https://linux.die.net/man/1/sg)

5. Make sure a ton of jobs aren’t failing right away. Permissions errors, job set up issues, and data issues are common causes.


For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
