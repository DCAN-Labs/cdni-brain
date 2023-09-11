# Interactive High Performance Computing (HPC) Resources 

Read: [Interactive HPC @ MSI](https://www.msi.umn.edu/content/interactive-hpc)


MSI offers several options to remotely access HPC resources. These will allow you to access stored data, run processing jobs, and other computing heavy tasks. **Note:** The first Wednesday of every month, MSI will be down for maintenance. Sometimes this spills over and can effect MSI performance for the whole week. 

* **Open OnDemand**: A web portal to access MSI's compute clusters in your browser, which can be used for accessing MSI files and running jobs.
    * Select *Interactive Apps* at the top and select which Desktop you would like
    * The *Persistent Desktop* will run until next maintenance day 
    * If selecting *Desktop* choose which cluster, how many resources, and the time you need 
    * Agate is good for visualization and GPU utilization
    * Mesabi is good for running long jobs 
    * Several other apps are available, including MATLAB and RStudio Server

![Open OnDemand Window](img/ood_example.jpeg)

* **Jupyter Notebooks:** A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R. Offered on OOD as a server.
    * Great for playing around with a database initially
    * Formalizing code here is not recommended
* **Terminal:** User input might come through a command line interface (i.e., system shell) while debugging a piece of software 
    * Great for working with codebases on VScode
* **srun:**  Submit an interactive job on the current HPC system (OOD or Terminal) to provide a command line or graphical interface to interactively run software.
    * Needed for running programs / visualizing data for more than 15 minutes on any system listed above


## PI Shares

Each PI has their own allocation on MSI. Damien Fair: `faird`; Eric Feczko `feczk001`; Oscar Miranda-Dominguez: `miran045`; Steve Nelson: `smnelson`; Anita Randolph: `rando149`. These allocations have storage and grid time allocated to them. As one account is more active, it becomes deprioritized relative to all other accounts on MSI. See [this page](fairshare.md) for more information about how prioritization works. 
