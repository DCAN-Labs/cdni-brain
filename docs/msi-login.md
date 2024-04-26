# Getting Started with Minnesota Supercomputing Institute (MSI)

To access the Minnesota Supercomputing Institute (MSI) system smoothly, confirm your eligibility, particularly if your tasks involve data analysis or processing. Visit  [Eligibility & Access Instructions](https://www.msi.umn.edu/content/eligibility-getting-access) for more detailed guidelines on eligibility and access requirements. Do not use MSI without completeing the steps outlined on this page.

## 2-Factor Authentification

You must set up Duo 2 Factor Authentification in order to use MSI. This provides an added layer of security and is mandatory for accessing any UMN internal site. Follow these [instructions](https://it.umn.edu/services-technologies/self-help-guides/duo-set-use-duo-security) to register and use Duo.

When you log in to MSI via a terminal you should see this prompt: 

```
By using this system you agree to adhere to MSI and UMN Acceptable Use Policies - refer to www.msi.umn.edu for details.
(your_x500@cluster.msi.umn.edu) Duo two-factor login for your_x500

Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-4690
 2. Phone call to XXX-XXX-4690

Passcode or option (1-2):
```

## Connecting to the UMN VPN

To access MSI from a network other than eduroam, you must first establish a secure connection using a Virtual Private Network (VPN). If you are using eduroam, you do not need to connect to the VPN. If you try to connect to MSI without the proper network/VPN connection, the OpenOnDemand page will not load. 

- Find detailed VPN instructions on the [MSI VPN](https://it.umn.edu/services-technologies/virtual-private-network-vpn) page.
- Scroll halfway down the page to discover installers for Cisco AnyConnect on different operating systems.
- The page will also explain the difference between split-tunnel, full-tunnel, and departmental. Split-tunnel is the default and preferred method of connecting unless an app requires full-tunnel (MSI doesn't).

**Cisco AnyConnect Installation:**

- Download and install Cisco AnyConnect based on your operating system.
- Follow the installation prompts to complete the setup.

**Connecting to UMN's VPN:**

- Launch Cisco AnyConnect on your device.
- Choose the "Split Tunnel VPN" option.
- Log in using your credentials.
- Although the VPN won't directly prompt you, you will have to dual authenticate with Duo every time.

Once connected, you will be able to log in to MSI.

## Logging in to MSI

See [this page](hpc.md) for how to log in to an [OnDemand](https://ondemand.msi.umn.edu/pun/sys/dashboard/batch_connect/sessions) Desktop and other ways to access MSI.

To connect via ssh in a terminal, you will have to have the SSH keys properly configured for the MSI cluster you are trying to connect to. [This page](https://www.msi.umn.edu/content/ssh-keys) has the current keys for the clusters and [this page](https://www.msi.umn.edu/support/faq/how-do-i-setup-ssh-keys) has instructions for setting up your SSH keys for the first time. There was an update to the ssh keys for the clusters in June of 2023 so if you still need to update your keys, [this page](https://www.msi.umn.edu/content/ssh-keys-renew-june-2023) has instructions on how to update the keys on your system. To learn more about SSH, see [this website.](https://www.cloudflare.com/learning/access-management/what-is-ssh/)

MSI can be accesed through any regular terminal but you must first log in to a login node: `ssh -Y <user>@login.msi.umn.edu`. A login node can be used to browse, view files, etc. When using a cluster to perform more advanced/computational heavy tasks, log in to one of MSI's clusters, like Agate or Mangi with `ssh -Y <USERNAME>@<cluster>.msi.umn.edu` or if you're already on a login node `ssh -Y agate or mangi`. More information about login vs compute nodes can be found on [this page.](partitions.md) When you use an OnDemand Desktop, you are automatically placed on a compute node with the resources listed on the Desktop creation page. You will still need to ssh into a cluster if you want to grab a srun.

You can also access MSI through VSCode, more information about how to do that can be found on [this page.](vscode.md)

If you are looking for access to an s3 bucket, you will need to have logged into MSI at least once. 

For additional guidance watch [MSI video tutorial](https://www.youtube.com/watch?v=PgD7WSI6CG4).

## Directory and File Permissions

To ensure the data and code created can be accessed by all, update your `.bashrc` with the following steps (this only needs to be done the first time you access MSI). See [here](https://www.digitalocean.com/community/tutorials/bashrc-file-in-linux) for some info on what a `.bashrc` is.

Open your `.bashrc` file with a text editor, e.g. `emacs ~/.bashrc`.
Set umask to 007. The umask is the default permission applied to the files you create. Permissions are how self, groups (like `faird`) and other users can be given read, write, and execute access. With 007, self and group members can be given those permissions but no one else (this could be anyone in the university). 
Close the file and type `source ~/.bashrc` into the terminal to apply the changes.
Your `.bashrc` is loaded each time you log in, so you only need to source it when you edit it mid-session. 

Here is a template for what your `.bashrc` should look like

```
# .bashrc startup script for login shells
#

# Set your umask.
# umask 077       # -- private, only you have access to your files
# umask 022     # -- anyone can read and execute your files
# umask 027     # -- only members of your group can read/execute your files
umask 007    # -- created files may take upto read/write/execute

# Set the prompt.
PS1="\u@\h [\w] % "

# Add your aliases here.
# alias s='ssh -X'

# Set your environment variables here.
# export VISUAL=vim

# Uncomment the if statement below to enable bash completion.
# if [ -f /etc/bash_completion ]; then
#  source /etc/bash_completion
# fi

# Load modules here
# module load
```

## Gaining Access
To gain access to MSI shares containing ABCD derivatives (`feczk001`, `miran045`, `rando149`, `smnelson`), you must be an approved collaborator on the UMN DUC.

* Create a [Login.gov account](https://www.login.gov/create-an-account/) 
* Use your login.gov credentials to [access the NDA](https://nda.nih.gov/user/login_required.html?originator=%2Fuser%2Fdashboard%2Fdashboard.html) - you’ll then be prompted to create an NDA account
* Email Borgne Raasch `braasch@umn.edu` your NDA username to be added to the next submission batch
* Outside collaborators should follow this link: ​​ [NDA - Adding Affiliated Institutions](https://docs.google.com/document/d/1w5BW14EHFSi4Lr1YDPm9CLTiy8JNdClt5KpKCOYnBH4/edit#heading=h.qdjbnp8qckwr)
* After Borgne confirms that you have been granted access to the DUC, email Luci Moore and Kim Weldon at `lmoore@umn.edu` and `kweldon@umn.edu` to granted access to `feczk001`, `miran045`, `rando149`, or `smnelson` and let Borgne know if you also need access to the `NGDR`. For more information on what the `NGDR` is, see [here](https://rc.umn.edu/project/neuroimaging-and-genetic-data-resources). It is where we store our ABCC BIDS inputs and derivatives.

