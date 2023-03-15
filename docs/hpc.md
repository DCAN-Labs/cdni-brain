# Interactive High Performance Computing (HPC) Resources 

Read: [Interactive HPC @ MSI](https://www.msi.umn.edu/content/interactive-hpc)


    MSI offers several options to remotely access HPC resources

* **NICE:** Connects a user to a graphical environment, which may have specialized GPUs for visualizing large datasets. 
    * Best for data visualization
    * Terminals and folders will close when the time limit is reached on the interactive session
    * Limited CPU / RAM allocation (2 cores / 16 GB max) makes it unsuitable for more intensive tasks such as preprocessing pipelines.
* **NX NoMachine:** Connects a user to a graphical environment, which may be used as a lightweight remote desktop for connecting to other MSI resources.
    * User’s session persists even if the browser or client is closed, and upon reconnection can be resumed from where it was left (session will still be terminated by MSI maintenance day 1st Wednesday of each month, or by other downtime).
    * Great to leave terminals and folders open for weeks
* **Jupyter Notebooks:** A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R.
    * Great for playing around with a database initially
    * Formalizing code here is not recommended
* **Terminal:** User input might come through a command line interface (i.e., system shell) while debugging a piece of software 
    * Great for working with codebases on VScode
* **srun:**  Submit an interactive job on the current HPC system (NICE, NX, or Terminal) to provide a command line or graphical interface to interactively run software.
    * Needed for running programs / visualizing data for more than 15 minutes on any system listed above