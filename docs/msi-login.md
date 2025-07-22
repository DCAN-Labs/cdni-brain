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

To connect via ssh in a terminal, you will have to have the SSH keys properly configured for the MSI cluster you are trying to connect to. [This page](https://www.msi.umn.edu/support/faq/how-do-i-setup-ssh-keys) has instructions for setting up your SSH keys for the first time but can be confusing so we have simplified the steps below. To learn more about what SSH is, see [this website.](https://www.cloudflare.com/learning/access-management/what-is-ssh/)

**Setting up SSH Keys**

In a terminal on your **local** computer, run these commands from your home directory:

```
ssh-keygen -t rsa -b 4096
cat ~/.ssh/id_rsa.pub >> temp.pub
```

- The `keygen` command will prompt you to enter a password to further increase the security of your SSH keys but you can just press enter for those prompts to skip that step. 

Upload the `temp.pub` file to your home directory on MSI via the files tab on OpenOnDemand.

In a terminal on **MSI**, run these commands from your home directory:

```
mkdir .ssh
cat ~/temp.pub >> .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
rm temp.pub
```

You could also create the authorized_keys file and directly copy and paste the ssh key from your local computer into that file, then run just the chmod command. 

MSI can be accessed through any regular terminal with this command: `ssh -Y <x500>@<cluster>.msi.umn.edu`. This will automatically place you onto a login node, which can be used to browse, view files, etc. When using a cluster to perform more advanced/computational heavy tasks, grab [an srun](slurm-params.md#srun) to enter a compute node. More information about login vs compute nodes can be found on [this page.](partitions.md) 

When you use an OnDemand Desktop, you are automatically placed on a compute node with the resources listed on the Desktop creation page. You will still need to ssh into a login node if you want to grab a srun.

You can also access MSI through VSCode, more information about how to do that can be found on [this page.](vscode.md)

If you are looking for access to an s3 bucket, you will need to have logged into MSI at least once. 

For additional guidance watch [MSI video tutorial](https://www.youtube.com/watch?v=PgD7WSI6CG4).

## Directory and File Permissions

To ensure the data and code created can be accessed by all, update your `.bashrc` with the following steps (this only needs to be done the first time you access MSI). See [here](https://www.digitalocean.com/community/tutorials/bashrc-file-in-linux) for some info on what a `.bashrc` is.

Open your `.bashrc` file with a text editor, e.g. `emacs ~/.bashrc`.
Set umask to 002. The umask is the default permission applied to the files you create. Permissions are how self, groups (like `faird`) and other users can be given read, write, and execute access. With 002, self and group members can be given those permissions but no one else (this could be anyone in the university). 
You will also need to add the path to the s3policy_bin, which you can read more about [here](s3.md#granting-bucket-access). 
Close the file and open a terminal to apply the changes.
Your `.bashrc` is loaded each time you open a new terminal. 

Here is a template for what your `.bashrc` should look like. You can add more as you use MSI more and determine what would be helpful. See [this page](roadblocks.md) for some potentially helpful .bashrc additions. 

```
# .bashrc startup script for login shells
#

# Set your umask.
umask 002  

# Set the prompt.
PS1="\u@\h [\w] % "

# Add your aliases here.
# alias s='ssh -X'

# Set your environment variables here.
# export VISUAL=vim
export PATH=/home/dhp/public/storage/s3policy_bin/:$PATH

# Uncomment the if statement below to enable bash completion.
# if [ -f /etc/bash_completion ]; then
#  source /etc/bash_completion
# fi

# Load modules here
# module load
```

## Gaining Access to Shares / Obtaining UMN Data Use Certification (DUC) Approval 
Your supervisor/mentor (or CDNI liaison for outside collaborators) will assist with adding you to the MSI shares you will need access to. The default share that new people are added to is 'faird' (this is where most of the commonly used scripts/pipeline/software is stored). 

Note that some shares contain ABCD data and therefore require that you be added to the Data Use Certification (DUC) for ABCD, administered by the NBDC ([see details](https://www.nbdc-datahub.org/data-access-process)). This includes:

- `feczk001`
- `miran045`
- `rando149`
- `smnelson`

Everyone whose primary affiliation is CDNI (i.e. their direct supervisor or mentor is someone in leadership) should be included on the UMN DUC - if you are new, please reach out to Kristina Hufnagle to confirm that your addition to the DUC has been both submitted and approved.

Collaborators can email Borgne Raasch `braasch@umn.edu` to confirm that they are included on the UMN DUC.


### OLD INFO TO REMOVE

* Create a [Login.gov account](https://www.login.gov/create-an-account/) 
* Use your login.gov credentials to [access the NDA](https://nda.nih.gov/user/login_required.html?originator=%2Fuser%2Fdashboard%2Fdashboard.html) - you’ll then be prompted to create an NDA account
* Email Borgne Raasch `braasch@umn.edu` your NDA username to be added to the next submission batch. Adding new usernames to the DUC only happens once a month so this process can take a while depending on how the timing lines up. 
* Outside collaborators should follow this link: ​​ [NDA - Adding Affiliated Institutions](https://docs.google.com/document/d/1w5BW14EHFSi4Lr1YDPm9CLTiy8JNdClt5KpKCOYnBH4/edit#heading=h.qdjbnp8qckwr)
* After Borgne confirms that you have been granted access to the DUC, please reach out directly to the PI whose share you would like to be added to and include proof that you've been added to the DUC (e.g. forward or share a screenshot of confirmation email from Borgne). PI's can find information about how to add someone to their share [here.](https://msi.umn.edu/manage-pi-group)
