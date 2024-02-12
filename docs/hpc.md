# High Performance Computing (HPC) Resources 

Read: [HPC @ MSI](https://www.msi.umn.edu/content/interactive-hpc)


MSI offers several options to remotely access HPC resources. These will allow you to access stored data, run processing jobs, and other computing heavy tasks. 

**Note:** The first Wednesday of every month, MSI will be down for maintenance. Sometimes this spills over and can effect MSI performance for the whole week. 

* **Terminal:** User input might come through a command line interface (i.e., system shell) while debugging a piece of software 
    * Great for working with codebases on VScode
* **srun:**  Submit an interactive job on the current HPC system (OOD or Terminal) via SLURM to provide a command line or graphical interface to interactively run software. For more information on SLURM see [this section](slurm.md)
    * Needed for running programs / visualizing data for more than 15 minutes on any system listed above


## PI Shares

Each PI has their own allocation on MSI. These allocations have storage and grid time allocated to them. As one account is more active, it becomes deprioritized relative to all other accounts on MSI. See [this page](fairshare.md) for more information about how prioritization works. 

* Damien Fair: `faird`
* Eric Feczko `feczk001`
* Oscar Miranda-Dominguez: `miran045`
* Steve Nelson: `smnelson`
* Anita Randolph: `rando149`

