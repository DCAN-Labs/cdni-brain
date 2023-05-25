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

# Connecting to MSI with VS Code

First, if you have not downloaded VS Code, [install it](https://code.visualstudio.com/download) on your local computer. 

There are several recommended extensions to download through the Extensions Marketplace

- Remote-SSH (needed to connect to MSI)

- Git Graph (a helpful tool for visualizing your git history of commits, merges, branches, etc)

- Python

- Pylance

- Jupyter

If you are not connected to the university network, [connect to a VPN](https://it.umn.edu/services-technologies/virtual-private-network-vpn)

Next you will need to setup SSH keys that will allow you to connect to MSI.

- Open a terminal on your local computer and run `ssh-keygen`

- Copy the contents of `~/.ssh/id_rsa.pub` 

- Open an OnDemand session and open a fresh terminal in your home directory

- In the `~/.ssh/` folder, create an `authorized_keys` file and paste in the ssh key you copied earlier

- Run `chmod 600 ~/.ssh/authorized_keys` after you add the key. This will ensure that the ssh server will see this file

- See [this link](https://www.msi.umn.edu/support/faq/how-do-i-setup-ssh-keys) for more help

Now you need to edit the ssh config file on your local computer to be able to connect to MSI servers.

- Copy the following text into your `~/.ssh/config` file on your local computer, changing USERNAME to your x500. The port numbers are arbitrary and can be changed freely. 

        XAuthLocation /opt/X11/bin/xauth
        ServerAliveInterval 60
        ServerAliveCountMax 120

        Host mangi.msi.umn.edu
        HostName mangi.msi.umn.edu
        User USERNAME
        LocalForward 12326 localhost:12326

        Host cn*
        ser USERNAME
        ProxyCommand ssh -L 12326:localhost:12326 -W %h:%p mangi.msi.umn.edu
        LocalForward 12326 localhost:12326
        ForwardX11Trusted yes

        Host mesabi.msi.umn.edu
        HostName mesabi.msi.umn.edu
        User USERNAME
        LocalForward 12325 localhost:12325
        ForwardX11Trusted yes

        Host acn*
        User USERNAME
        ProxyCommand ssh -L 12325:localhost:12325 -W %h:%p mesabi.msi.umn.edu
        LocalForward 12325 localhost:12325
        ForwardX11Trusted yes

Time to actually connect to MSI!

 - Under the View tab, open the Command Palette and search for Remote SSH: Connect to Host. Mangi and Mesabi should both be listed as options.

 ![Command Palette](command_palette.png)

 ![Remote SSH: Connect to Host](remote_ssh.png)

 ![Clusters](msi_clusters.png)

 - Select a cluster, this is open a new window that will prompt you for the ssh password you set up when you set up your keys

 - In the bottom right corner a box with pop up that says "Setting up SSH Host cluster.msi.umn.edu (details)", click details to complete the DUO authentification 

 - Now you will need to install the same extensions as before but on the remote server  
    
    - Refer to the instructions as earlier, just make sure you are connected to MSI

It is best to not run VS Code on a login node, as it takes up a decent amount of CPU and memory to run, so instead connect to a compute node.

- Once you are connected to MSI, open a new terminal and grab an srun

    - `srun --time=8:00:00 --ntasks=2 --mem=32G --tmp=20G --pty bash -l`

- Make a note of the compute node you are allocated (`cnXXX` or `acnXXX`)

- Open the command palette again and select Remote-SSH: Connect to Host and enter the compute node 

- You'll be prompted to enter your ssh password and duo again

- It will also ask you to save the host fingerprint, select Continue/yes then enter your ssh password again to connect

When you are done using MSI, it is good practice to close the connection

- Click the box in the very bottom left side of the screen that says SSH: cluster.msi.umn.edu

- A command palette box will pop up in the top middle of your screen. Select Close Remote Connection.

If your session keeps timing out before it connects, open VSCode settings, search for "connectTimeout" and increase "Remote SSH: Connect Timeout" to 60.