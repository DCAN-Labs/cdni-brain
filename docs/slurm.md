# Slurm

Read:


* [Slurm @ MSI documentation ](https://www.msi.umn.edu/slurm)
* [Slurm official site](https://slurm.schedmd.com/documentation.html)
* [Slurm commands cheat sheet](https://slurm.schedmd.com/pdfs/summary.pdf)

    **Slurm** is MSI’s job scheduling system. It is responsible for managing the allocation of computing resources among MSI users and user groups. In the DCAN labs, most data processing tasks are executed via Slurm job submissions, in the form of **batch scripts** submitted with the **sbatch **command. A batch script is a text file that specifies both the resources (e.g. CPUs, RAM, time) and the commands to be run for a processing job.


    Slurm also has “interactive” jobs, which allow access to compute resources directly from the terminal, as opposed to batch job submissions which run in the background.

    * **NOTE**: Login nodes, e.g. “ah1001”, “ln1001” should only be used for minimal tasks like batch script submission, filesystem browsing, small file transfers, and text editing.  Jobs requiring more time or resources should be run via sbatch submission, srun, or interactive session with dedicated compute resources. **Processes on login nodes are automatically terminated after 15 minutes.** 


    Also available are various commands for job accounting, job management, and environment configuration. (See cheat sheet linked below)


    **A table summarizing some commands that can be used inside Slurm job scripts ([sruns](#4-1-srun-immediately-run-a-command-using-the-specified-compute-resources) and [sbatchs](#4-2-sbatch-copies-the-script-in-an-internal-storage-and-then-uploads-it-on-the-compute-node-when-the-job-starts))**. The first four commands are required, while the other commands are optional. See [here](https://slurm.schedmd.com/sbatch.html) for a larger list of options. For parameter optimization, [refer to using seff in this section](#15-optimizing-sbatch-parameters).


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
   <td><em>Required for sbatch.</em> Specifies how the Slurm file should be read (by the bash interpreter). A statement like this is required to be the first time of a Slurm script
   </td>
  </tr>
  <tr>
   <td>-t 8:00:00
<p>
--time=8:00:00
   </td>
   <td><em>Required.</em> Specifies the maximum limit for how long the job will be allowed to run 
   </td>
  </tr>
  <tr>
   <td>--ntasks=8
   </td>
   <td>Required: Specifies the number of processors (cores) that will be reserved for the job
   </td>
  </tr>
  <tr>
   <td>-A, --account=<em>share</em>
   </td>
   <td>Optional, but highly recommended: Charge resources used by this job to the specified account or default account. The account name may be changed after job submission using the scontrol command. To choose an optimal share, see the Fairshare explanation <a href="#5-fairshare">here</a>.
   </td>
  </tr>
  <tr>
   <td>-c 3
<p>
--cpus-per-task=3
   </td>
   <td>Optional: Without this option, the controller will just try to allocate one processor per task. For instance, consider an application that has 4 tasks, each requiring 3 processors. If our cluster is comprised of quad-processor nodes and we simply ask for 12 processors, the controller might give us only 3 nodes. However, by using the --cpus-per-task=3 options, the controller knows that each task requires 3 processors on the same node, and the controller will grant an allocation of 4 nodes, one for each of the 4 tasks.
   </td>
  </tr>
  <tr>
   <td>--mem=10g
   </td>
   <td>Optional: Specifies the maximum limit for memory usage for the entire job. This job will die if the application tries to use more than 10GB of memory*
   </td>
  </tr>
  <tr>
   <td>--mem-per-cpu=<em>&lt;size>[units]</em>
   </td>
   <td>Optional: Minimum memory required per allocated CPU
   </td>
  </tr>
  <tr>
   <td>--tmp=10g
   </td>
   <td>Optional: Specifies 10GB of temporary disk space will be available for this job in /tmp. Should only be used if you are specifying /tmp folders for inputs, outputs, or working directories*
   </td>
  </tr>
  <tr>
   <td>--mail-type=ALL
   </td>
   <td>Optional: Specifies which events will trigger an email message. Other options here include NONE, BEGIN, END, and FAIL. Not recommended for 100+ subject jobs
   </td>
  </tr>
  <tr>
   <td>--mail-user=x500@umn.edu
   </td>
   <td>Optional: Specifies the email address that should be used when the Slurm system sends message emails. Make sure to double check each time. People frequently get emails from others due to other people copying and running their sbatches
   </td>
  </tr>
  <tr>
   <td>-p small,mygroup 
<p>
--partition=small,mygroup 
   </td>
   <td>Optional: Specifies the partition to be the “small” partition, or a partition named “mygroup”. The job will start at the earliest time one of these partitions can accommodate the job. You must be logged into the correct <a href="#2-interactive-high-performance-computing-hpc-resources">HPC resource</a> to access corresponding partitions. For more info, see <a href="#3-partitions">here</a>.
   </td>
  </tr>
  <tr>
   <td>--gres=gpu:v100:2
<p>
-p v100
   </td>
   <td>Optional: Request two v100 GPUs for a job submitted to the V100 group
   </td>
  </tr>
</table>



*`--mem` is the amount of RAM (random access memory) on a CPU (central processing unit), while `--tmp` indicates the amount of temporary storage that you can utilize for a job. With whatever storage amount is specified for `--tmp`, that amount will be created for you within the /tmp folder to output your processing derivatives. 

**Other useful commands:**

1. View all jobs submitted by a given user:  `squeue -u username` (one can also use `squeue -al --me` when trying to determine specific jobs for their own account) or `squeue -A group_name` to check the queue for all groups one is under to determine which to submit under

2. Cancel a submitted job: `scancel jobID_number`
    - scancel can also be used for job arrays by listing the job ID numbers as a comma separated list.

3. Display accounting data for all jobs and job steps in the Slurm job accounting log or Slurm database: `sacct` 
    - See the below command to check the status of a job even after it has exited. JOBID_ARRAY can also just be the JOBID, if you didn't submit an array
        `sacct -X -j JOBID_ARRAY# -o JobID,NNodes,State,ExitCode,DerivedExitCode,Comment`
    - Check the jobs you have in queue by using squeue -al --me

4. Change job account (each PI in the lab has their own Slurm group account with its own allocation of resources and queue priority. It is sometimes useful to change accounts to distribute resource requests for large processing jobs, or when an account has low queue priority due to heavy usage) : `scontrol update JobId=#### Account=new_group`
    - An example command has _Job 234293_ originally submitted under default account _miran045_, and to change it to _feczk001 _one would use: scontrol update JobId=234293 Account=feczk001

5. To change a slurm job partition use: `scontrol update JobId=#### Partition=new_partition` 
    - An example command has _Job 234293_ originally submitted with the partition _msismall_, and to change it to _msigpu _one would use: scontrol update JobId=234293 Partition=msigpu

6. To change the amount of time a slurm job runs for, use: `scontrol update JobId=#### EndTime=HH:MM:SS` 
    - To find time information, first use `scontrol show JobId=####`
    - An example command has _Job 234293_ originally submitted at the following time for 96 hours: _StartTime=2022-08-29T13:04:45_, and to change it to 48 hours one would use: scontrol update JobId=234293 EndTime=2022-08-31T13:04:45