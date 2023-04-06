# Interactive High Performance Computing (HPC) Resources 

Read: [Interactive HPC @ MSI](https://www.msi.umn.edu/content/interactive-hpc)


MSI offers several options to remotely access HPC resources

![Open OnDemand Window](img/ood_example.jpeg)

* **Open OnDemand**: A web portal to access MSI's compute clusters in your browser, which can be used for accessing MSI files and running jobs.
    * Select *Interactive Apps* at the top and select which Desktop you would like
    * The *Persistent Desktop* will run until next maintenance day 
    * If selecting *Desktop* choose which cluster, how many resources, and the time you need 
    * Agate is good for visualization and GPU utilization
    * Mesabi is good for running long jobs 
    * Several other apps are available, including MATLAB and RStudio Server
* **Jupyter Notebooks:** A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R. Offered on OOD as a server.
    * Great for playing around with a database initially
    * Formalizing code here is not recommended
* **Terminal:** User input might come through a command line interface (i.e., system shell) while debugging a piece of software 
    * Great for working with codebases on VScode
* **srun:**  Submit an interactive job on the current HPC system (OOD or Terminal) to provide a command line or graphical interface to interactively run software.
    * Needed for running programs / visualizing data for more than 15 minutes on any system listed above