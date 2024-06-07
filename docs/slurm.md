# Slurm Commands

Read:

* [Slurm @ MSI documentation ](https://www.msi.umn.edu/slurm)
* [Slurm official site](https://slurm.schedmd.com/documentation.html)
* [Slurm commands cheat sheet](https://slurm.schedmd.com/pdfs/summary.pdf)

**Slurm** is MSI’s job scheduling system. It is responsible for managing the allocation of computing resources among MSI users and user groups. In the CDNI, most data processing and analysis tasks are executed via Slurm job submissions, in the form of **batch scripts** submitted with the `sbatch` command. A batch script is a text file that specifies both the resources (e.g. CPUs, RAM, time) and the commands to be run for a processing job. Slurm jobs can be used for any process that requires more computing resources than is available within a normal terminal. 

Slurm also has “interactive” jobs (srun), which allow access to compute resources directly from the terminal, as opposed to batch job submissions which run in the background (sbatch).

<div class="admonition note">
   <p class="first admonition-title">Note</p>
   <p class="last">
      Processes on login nodes are automatically terminated after 15 minutes.
   </p>
</div>

Read more about login vs compute nodes on [this page](partitions.md#nodes)

Also available are various commands for job accounting, job management, and environment configuration. (See cheat sheet linked below)

## Job Parameters

Below is a table summarizing some commands that can be used inside Slurm job scripts (see [sruns and sbatch](slurm-params.md)). The first four commands are required, while the other commands are optional. See [here](https://slurm.schedmd.com/sbatch.html) for a larger list of options. For parameter optimization, [refer to using seff in this section](optimizing.md).


<table>
  <tr>
   <td>
<strong>Slurm command</strong>
   </td>
   <td><strong>Effect</strong>
   </td>
  </tr>
  <tr>
   <td>#!/bin/bash -l  
   </td>
   <td><em>Required for sbatch:</em> Specifies how the Slurm file should be read (by the bash interpreter). <br /> A statement like this is required to be the first time of a Slurm script
   </td>
  </tr>
  <tr>
   <td>-t 8:00:00
<p>
--time=8:00:00
   </td>
   <td><em>Required:</em> Specifies the maximum limit for how long the job will be allowed to run 
   </td>
  </tr>
  <tr>
   <td>--ntasks=8
   </td>
   <td><em>Required:</em> Specifies the number of processors (cores) that will be reserved for the job
   </td>
  </tr>
  <tr>
   <td>--nodes=2
   </td>
   <td><em>Optional:</em> Specifies the number of nodes that the job will run on
   </td>
  </tr>
  <tr>
   <td>--ntasks-per-nodes=4
   </td>
   <td><em>Optional:</em> Specifies the number of core that each node will have reserved for the job
   </td>
  </tr>
  <tr>
   <td>-A, --account=<em>share</em>
   </td>
   <td><em>Optional:</em> Charge resources used by this job to the specified account. <br /> The account may be changed after job submission using the scontrol command. <br /> To choose an optimal share, see the Fairshare explanation <a href="fairshare.md">here</a>.
   </td>
  </tr>
  <tr>
   <td>-c 3
<p>
--cpus-per-task=3
   </td>
   <td><em>Optional:</em> Choose how many processors on a node are needed for a task. <br /> By default, SLURM will just try to allocate one processor per task. Say a job had 4 tasks <br /> that each need 3 processors and the cluster had a quad-processor node, if you <br /> simply ask for 12 processes, SLURM will only give 3 nodes. With this option, <br /> SLURM knows that each task requires 3 processors on the same node, and will give one node per task.
   </td>
  </tr>
  <tr>
   <td>--mem=10g
   </td>
   <td><em>Optional:</em> Specifies the maximum limit for memory usage for the entire job. <br /> This job will die if the application tries to use more than 10GB of memory*
   </td>
  </tr>
  <tr>
   <td>--mem-per-cpu=<em>&lt;size>[units]</em>
   </td>
   <td><em>Optional:</em> Minimum memory required per allocated CPU. 
   </td>
  </tr>
  <tr>
   <td>--tmp=10g
   </td>
   <td><em>Optional:</em> Specifies 10GB of temporary disk space will be available for this job in /tmp. <br /> Should only be used if you are specifying /tmp folders for inputs, outputs, or working directories*
   </td>
  </tr>
  <tr>
   <td>--mail-type=ALL
   </td>
   <td><em>Optional:</em> Specifies which events will trigger an email message. Other options here <br /> include NONE, BEGIN, END, and FAIL. Not recommended for 100+ subject jobs
   </td>
  </tr>
  <tr>
   <td>--mail-user=x500@umn.edu
   </td>
   <td><em>Optional:</em> Specifies the email address that should be used when the Slurm system sends message emails. <br /> Make sure to double check each time. People frequently get emails from others <br /> due to other people copying and running their sbatches
   </td>
  </tr>
  <tr>
   <td>-p small,mygroup 
<p>
--partition=small,mygroup 
   </td>
   <td><em>Optional:</em> Specifies the partition to be the “small” or "mygroup" partition. <br /> The job will start at the earliest time one of these partitions can accommodate the job. <br /> You must be logged into the correct cluster access corresponding partitions. For more info, see <a href="paritions.md">here</a>.
   </td>
  </tr>
  <tr>
   <td>--gres=gpu:v100:2
<p>
-p v100
   </td>
   <td><em>Optional:</em> Request two v100 GPUs for a job submitted to the V100 group
   </td>
  </tr>
</table>



*`--mem` is the amount of RAM (random access memory) on a CPU (central processing unit), while `--tmp` indicates the amount of temporary storage that you can utilize for a job. With whatever storage amount is specified for `--tmp`, that amount will be created for you within the /tmp folder to output your processing derivatives. 

## Job Status

This section has commands that can be used to check on the status of your job submission, cancel a job, or change the resources of your job. 

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

[This page](https://slurm.schedmd.com/sacct.html) has more options for using `sacct`.

### scontrol

`scontrol update JobId=#### Account=new_group`: change job account for a submitted job

- Each PI in the lab has their own Slurm group account with its own allocation of resources and queue priority. It is sometimes useful to change accounts to distribute resource requests for large processing jobs, or when an account has low queue priority due to heavy usage 

- Example command for moving _Job 234293_ that was originally submitted under _miran045_ and change it to _feczk001_: `scontrol update JobId=234293 Account=feczk001`

`scontrol update JobId=#### Partition=new_partition`: change a SLURM job parition

- Example command for moving _Job 234293_ that was originally submitted with the _msismall_ parition and change it to _msiqgu_: `scontrol update JobId=234293 Partition=msigpu`

**NOTE**: `scontrol` cannot be used to change between agate and mesabi paritions once a job has been submitted. It can be used to change from agate/mesabi to [federated paritions](partitions.md)

`scontrol update JobId=#### EndTime=HH:MM:SS`: change the amount of time a SLURM job runs

- Example command for moving  _Job 234293_ that was originally submitted at the following time for 96 hours: _StartTime=2022-08-29T13:04:45_ and change it to 48 hours:  `scontrol update JobId=234293 EndTime=2022-08-31T13:04:45`

`scontrol show JobId=####`: find time information for a job


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).