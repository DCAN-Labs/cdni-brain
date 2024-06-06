# sbatch/srun Parameters 

srun and sbatch are used for running jobs on MSI. srun is for running more interactive jobs, where you are running commands in the terminal. sbatch is for submitting job scripts, where you can still work on MSI while the job is running in the background. 

## srun

Used to immediately run a command using the specified compute resources

Read: [Interactive queue use with srun @ MSI](https://www.msi.umn.edu/content/interactive-queue-use-srun)

1. **srun** is primarily useful for jobs that will take longer than 15 minutes to run on a [login node](partitions.md#nodes) or need more computing resources (i.e. RAM). They are also very useful for loading a Matlab session (or something similar) with sufficient resources. 
2. This will allow you to work directly in a terminal, allowing you to run resource intensive software on it. You get the outputs in your terminal and you cannot write other commands until it is finished. (If you append the “&” symbol to the command it will execute in the background, allowing continued use of the current terminal.)
3. If you disconnect, you will lose control over srun jobs, or they might be killed (depending on whether they use stdout or not). They will also be killed if the machine to which you connect to submit jobs is rebooted.  
4. Before grabbing an srun, you need to make sure you are ssh'd into one of the clusters: `ssh -Y agate/mangi`.
5. Ex: `srun --time=8:00:00 --mem-per-cpu=8GB --cpus-per-task=4  -A feczk001 --tmp=20gb  -p interactive --x11 --pty bash`
    * This interactive job is grabbing 8 hours on 4 cpus on the interactive partition with 8 gigabytes of memory per cpu, 20gb of temporary storage total, x11 enabled, and the ability to use your terminal, utilizing the feczk001 account’s allocated resources.
    * Use `groupquota` to check which account you are currently using. If the account you specified in the interactive job does not match the account listed when you run `groupquota`, then use `sg {share}` to switch to the specified account. Then use `groupquota` again to make sure you are now on the correct share.
    * x11 forwarding (`--x11`) enables X11 graphical apps to render correctly via remote connection.

## sbatch

This version of job submission copies the script in an internal storage and then uploads it on the compute node when the job starts.

Read: [Job Submission & Scheduling @ MSI](https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm)

The job script needs to stay the same until the job starts to produce the correct results. If you make changes to a script before the job starts, those edits will be reflected in the job. Some of the possible parameters used in sbatch scripts can be found on [this page.](slurm.md#job-parameters)

1. Results are written out as your script specifies. `stdout` and `stderr` will be outputted if the `-o` and `-e` flags are specified, and you can submit other commands right away.
2. An sbatch job is handled by Slurm; you can disconnect (not run interactively), kill your terminal, etc. with no consequence
3. To run an sbatch, use this command: `sbatch scriptname.sh`

    1. Example sbatch layout: 

    ![singularity_example](img/singularity_image.png)

    2. The line `#!/bin/bash -l` in needed to indicate that this is a bash script.
    3. The name of the job is “_abcd-xcp_”, which is a descriptive name that will appear when running `squeue -al --me` in the command line. 
    4. It will send all types of mail to the specified email address “lundq163@umn.edu”
    5. The resources utilized by this sbatch job include:
        * a maximum time of 8 hours 
        * 8 cpus per task 
        * queueing on the `msismall` partition 
        * 160 gigabytes of total RAM 
        * utilizing the account _rando149_’s queue
    6. There are full paths to a directory that will hold both `stdout` and `stderr` files, which are listed above as `output_logs/xcp_full_%A.out` and `output_logs/xcp_full_%A.err`. The `%A` signifies that the job ID number will be added to the filename.
        * Note: make sure that the _output_logs_ directory exists prior to job submission, if you choose to use one. You can also change that path or not have a sub folder altogether if you want. It is recommended to specify the full path.
        * If you are submitting a sbatch for a [pipeline wrapper](wrappers.md), you can use `output_logs/xcp_full_%A_%a.err` for the output logs path. The `%a` indicates the array number that is being submitted.
    7. All of the text after the #SBATCH parameters are the commands needed to run an XCP-D [singularity container](containers.md). This is one example of what can be in an sbatch, and it can be substituted for any command that may need to be run via an sbatch. Read more about the XCP-D specific flags [here.](https://xcp-d.readthedocs.io/en/latest/usage.html)
        1. The input and output directory paths are bound to the container’s filesystem using the `-B` syntax. There is also a path to a `.sif` file that will utilize the jobs resources in order to produce the desired outputs using what's in the input directory. This .sif file is a [singularity image](containers.md) that is being run on the specified input files.   
            * Note: these input, output and singularity image paths need to exist prior to running the sbatch. 
            * The input file is binded with the `:ro` option, which indicates it is read only. This is done to ensure that running the command won't accidentally alter the input file. 
    8. If an sbatch job is running for >3 minutes, there are most likely no errors in the run command, and job doesn't need to be closely monitored before completion.

## Continuous Job Submission
 
1. If a submission is above 2000 jobs (slurm’s max in-queue number of jobs), or you would like to space out job submission, use the continuous submitter.

    * Path to continuous submitter: `/home/faird/shared/code/internal/utilities/SLURM_wrappers/continuous_submitter`

    * The code can be found on GitHub [here.](https://github.com/DCAN-Labs/SLURM_wrappers/tree/main/continuous_submitter)

    * This command needs to be run on a persistent desktop, or at least a desktop with as many hours as it will take to submit all the jobs based on how far apart the submission interval is. 

    * Below is an example of what the submitter command will look like:

```
python continuous_slurm_submitter.py --partition small,amdsmall --job-name abcd-hcp-pipeline_full 
--log-dir /path/to/cont_slurm_submitter_logs --run_folder /path/to/project/folder/run_files.abcd-hcp-pipeline_full/ --n_cpus 8 --time_limit 48:00:00 --total_memory 20 --tmp_storage 100 --array-size 1000 --submission-interval 300 --account-name feczk001 --mail_user x500@umn.edu
```

The `array-size` is how many jobs you are submitting at once. The `submission-interval` is the amount of time in minutes to wait until submitting another array of jobs. 

3. Check the [fairshare](fairshare.md) to know which account to use for processing.
4. If you have a command or script that outputs to a different group than your primary MSI group (i.e. the group with your home directory), you can use `sg` to run as the group that matches the output directory instead. Recommended for abcd-hcp-pipeline, infant-abcd-bids-pipeline, and nhp-abcd-bids-pipeline to avoid permission errors in the FreeSurfer stage of the pipeline.

    * Example for when your output directory is on faird: `sg faird -c "singularity run <...>"`

    * This includes batch scripts. (e.g. `sg faird -c "sbatch sbatchscript.sh"`)

    * More info on sg [here](https://linux.die.net/man/1/sg)

5. Make sure a ton of jobs aren’t failing right away. Permissions errors, job set up issues, and data issues are common causes.
