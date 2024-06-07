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

## Storage

Each PI has their own allocation on MSI, which includes storage and grid time. As one account becomes more active, it becomes deprioritized relative to all other accounts on MSI. See [this page](fairshare.md) for more information about how prioritization works. 

* Damien Fair: `faird`
* Eric Feczko `feczk001`
* Oscar Miranda-Dominguez: `miran045`
* Steve Nelson: `smnelson`
* Anita Randolph: `rando149`
* Brenden Tervo-Clemmens: `btervocl`
* Bart Larsen: `bart`


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).