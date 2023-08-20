# Minnesota Supercomputing Institute (MSI) First Login Steps

To begin accessing the Minnesota Supercomputing Institute (MSI) system, follow these steps to ensure a smooth and successful login process:

If you plan to analyze, process, or access data, you need access to MSI. Instructions for eligibility and access can be found at: [Eligibility & Access Instructions](https://www.msi.umn.edu/content/eligibility-getting-access).

## Establish VPN

To access MSI from outside the university area, you must first establish a secure connection using a Virtual Private Network (VPN). Here is a [link](https://www.msi.umn.edu/support/faq/why-cant-i-ssh-msi-machine) for VPN instructions to access MSI. Install Cisco AnyConnect by visiting the [VPN page](https://it.umn.edu/services-technologies/virtual-private-network-vpn). Choose the appropriate installer for your operating system. Remember to select the "Split Tunnel VPN" option when logging in to AnyConnect.

NOTE: If you already have VPN on your computer for another institution, you will need to complete the following steps:

* Type "tc-vpn-1.umn.edu" in the box
* Choose "anyconnect-UofMvpnfull" from the group
* Enter your username and password
* This installs required setups for VPN, after which you will have a dropdown with UMN choices and you can use "split tunnel" moving forward

If you receive an error message the first time, simply re-open Cisco.

## Registering for Duo Security

To enhance security when accessing online information, you must set up Duo Security. This added layer of security is mandatory for accessing UMN internal sites. Follow these [instructions](https://it.umn.edu/services-technologies/self-help-guides/duo-set-use-duo-security) to register and use Duo.

## Logging In to MSI

After connecting to MSI use the command `ssh -Y <USERNAME>@mesabi.msi.umn.edu` on [OnDemand](https://ondemand.msi.umn.edu/pun/sys/dashboard/batch_connect/sessions) Desktop terminal.

MSI can also be accesed through any regular terminal but you must first log in to a login node: `ssh -Y <user>@login.msi.umn.edu`. A login node can be used to browse, view files, etc.

When using a cluster to perform more advanced tasks, log in to one of MSI's clusters, like Mesabi or Mangi or if from a login node `ssh -Y <user>@mesabi or mangi`.

For additional guidance watch [MSI video tutorial](https://www.youtube.com/watch?v=PgD7WSI6CG4).

## Permissions

To ensure the data and code created can be accessed by all, update your bashrc with the following steps (this only needs to be done the first time you access MSI). See [here](https://www.digitalocean.com/community/tutorials/bashrc-file-in-linux) for some info on what a "bashrc" is.

Open your bashrc file with a text editor, e.g. `emacs ~/.bashrc`.
Set umask to 0007. The umask is the default permission (self, group, like faird, and all users can be given read, write, and execute access) applied to the files you create. 0007 gives read, write, and execute access to you and all group members, but no one else (this could be anyone in the university). See here for details.
Close the file and type `source ~/.bashrc` into the terminal to apply the changes.
Your bashrc is loaded each time you log in, so you only need to source it when you edit it mid-session. 

## Gaining Access
To gain access to MSI shares containing ABCD derivatives (feczk001, miran045, rando149, smnelson), you must be an approved collaborator on the UMN DUC.

* Create a [Login.gov account](https://www.login.gov/create-an-account/) 
* Use your login.gov credentials to [access the NDA](https://nda.nih.gov/user/login_required.html?originator=%2Fuser%2Fdashboard%2Fdashboard.html) - you’ll then be prompted to create an NDA account
* Email Borgne Raasch `braasch@umn.edu` your NDA username to be added to the next submission batch
* Outside collaborators should follow this link ​​ [NDA - Adding Affiliated Institutions](https://docs.google.com/document/d/1w5BW14EHFSi4Lr1YDPm9CLTiy8JNdClt5KpKCOYnBH4/edit#heading=h.qdjbnp8qckwr)