# High-Performance Computing (HPC) Resources 

<div class="admonition attention">
    <p class="first admonition-title">Attention</p>
    <p class="last">
        Please be aware of the deprecation of Mesabi starting 5 June 2024
    </p>
</div>

Read: [HPC @ MSI](https://www.msi.umn.edu/content/interactive-hpc)

MSI offers several options for remotely accessing HPC resources. These allow you to access stored data, run processing jobs, and perform other computing-intensive tasks. 

**Please be aware that MSI undergoes maintenance on the first Wednesday of each month.** Occasionally, this may extend and affect MSI performance for the entire week, both before and after Wednesday. 


## Open OnDemand

This is a web portal for accessing MSI's compute clusters in your browser. You can use it to access MSI files and run jobs.

* Select *Interactive Apps* at the top and select which Desktop you would like:
* The *Persistent Desktop* should run until the next maintenance day 
* If selecting *Desktop* choose which cluster, how many resources, and the time you need 
* Agate is good for visualization and GPU utilization
* Mangi and Agate are good for running long jobs 
* Several other apps are available, including MATLAB and RStudio Server
    
![Open OnDemand Window](img/ood_example.jpeg)

## Jupyter Notebooks

A web portal for notebook-based computing in the browser, it can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. It currently supports Python 2, Python 3, and R. It is offered on OOD as a server.

* Great for playing around with a database initially
* Formalizing code here is not recommended

## Secure Shell (SSH)

Your computer can communicate with MSI resources via SSH. It is a secure way for administrators to access and manage remote computers over the internet, offering protection for data and allowing tasks like logging in, transferring files, and executing commands.
More information about SSH can be found [here](https://it.umn.edu/services-technologies/resources/use-secure-shell-ssh) on the MSI page. 

## srun 

To facilitate the execution of programs or visualization of data lasting more than 15 minutes on any of the systems mentioned above, please consider submitting an interactive job via SLURM. This will allow you to access a command line or graphical interface on the current HPC system (OOD or Terminal) for interactive software execution. For more detailed information on SLURM, you can refer [to this section](slurm.md).

## SBATCH

SBATCH is a tool used at the Minnesota Supercomputing Institute (MSI) to send tasks to the Slurm system for processing. Users create scripts detailing resource needs like time, processors, memory, and email notifications. These scripts are then submitted using sbatch, ensuring efficient job management and execution within MSI's computing environment. Users can also check job status with `squeue -u username` and cancel jobs with `scancel jobIDnumber`. For more information, refer to the [Job Submission and Scheduling (Slurm)](https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm)

## Tier 1 Storage

This is the primary filesystem that is used for interactive file manipulation. Most of the PI shares have a quota of 20TB (`btervocl` and `bart` have less since MSI updated their default quotas). 

- If a group reaches 90% capacity, it becomes locked down, meaning no new files can be created and functionality will slow down.
- It is especially important to keep the `faird` share clean as this is the default share that new users and outside collaborators are given access to. 

The only data stored on tier 1 should be data that is being actively used. 

- If you want to store more than 1TB of data (or more than 500GB of data on `faird`), please fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLSd1QI_Hmi3khwITVctnaDJYY2M1NegsAWYPR6AXoodUCrrpZw/viewform).

MSI takes "snapshots" every night of the tier 1 space and stores them for a week. This means if you want to see an older version of a file or need to recover something that was accidentally deleted, you can enter the hidden `.snapshot` folder that lives in every directory to see the backups. Please keep in mind that these backup **are only stored for a week**.

Each PI has their own allocation on MSI, which includes storage and grid time. As one account becomes more active, it becomes deprioritized relative to all other accounts on MSI. See [this page](fairshare.md) for more information about how prioritization works. 

* Damien Fair: `faird`
* Eric Feczko `feczk001`
* Oscar Miranda-Dominguez: `miran045`
* Steve Nelson: `smnelson`
* Anita Randolph: `rando149`
* Brenden Tervo-Clemmens: `btervocl`
* Bart Larsen: `bart`

Each share on MSI follows a similar directory structure:

```
|--home
   |--<group>
      |--shared
         |--code
            |--external
                |--pipelines
                |--utilities
                |--analysis
                |--envs
            |--internal
                |--same subdirs as external
         |--projects
         |--data
      |--<old_home_dirs>
```

* If you are working on something that other people might need access to, it is best to create a directory under `projects`. 
* Each user has their own home directory that is separate from the PI's share. If you were added to MSI before the summer of 2024, you also still have your old home directory under the PI share. These are read and write protected directories, meaning only you have access to read and write to them. You have the ability to open them up, but it is not recommended.

## Scratch Space

`/scratch.global/` is an open filesystem that is not connected to a PI share. Each group still has a quota, but here it is 40TB and 10 million files. Data in global scratch is deleted after 30 days (unclear if this means 30 days after the data was first put into scratch or if this is 30 days after the data was last modified). This is helpful for if you are running jobs that produce large outputs that you want access to right away (instead of pushing them to the s3) or when you only need the outputs stored temporarily. 

Most users create their own x500-named working directory, i.e. `/scratch.global/faird/` to work under.

You can read more about scratch space [here on MSI's website](https://msi.umn.edu/about-msi-services/data-storage/high-performance-storage/scratch-storage)

For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).