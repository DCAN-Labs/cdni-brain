# Getting Started with Minnesota Supercomputing Institute (MSI)

If you are going to be processing, analyzing, or otherwise interacting with MRI data, you will need to have access to MSI. Visit  [Eligibility & Access Instructions](https://www.msi.umn.edu/content/eligibility-getting-access) for more detailed guidelines on eligibility and access requirements. This page outlines the steps needed to access MSI. 

## Duo 2-Factor Authentification

You must set up Duo 2 Factor Authentification in order to use MSI and any internal UMN site. This provides an added layer of security. UMN provides a [Duo Guide](https://it.umn.edu/services-technologies/self-help-guides/duo-set-use-duo-security) which provides instructions for how to register and use Duo.

If you log in to MSI via a terminal you will see and complete this prompt to authenticate: 

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
- Choose the "UMN - Split Tunnel - General Access VPN" option.
- Log in using your credentials.
- Although the VPN won't directly prompt you, you will have to dual authenticate with Duo every time.

Once connected, you will be able to log in to MSI.

## Connecting to MSI

**Remote Desktop**

One way to connect to MSI is via an [OnDemand Desktop](https://ondemand.msi.umn.edu/pun/sys/dashboard/batch_connect/sessions), which opens a virtual machine in your browser and allows you to interact with MSI as a typical computer. See our [Tier 1 Resources page](hpc.md#open-ondemand) for more information.

When you use an OnDemand Desktop, you are automatically placed on a compute node with the resources listed on the Desktop creation page. You will still need to ssh into a login node if you want to grab a srun.

**Local Terminal**

You can also directly connect to MSI via your computer's terminal. To connect via ssh in a terminal, you will have to have the SSH keys properly configured for the MSI cluster you are trying to connect to. MSI has a [guide for setting up SSH keys](https://www.msi.umn.edu/support/faq/how-do-i-setup-ssh-keys) for the first time but can be confusing so we have simplified the steps below. You can [learn more about what SSH is here.](https://www.cloudflare.com/learning/access-management/what-is-ssh/)

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

MSI can be accessed through any regular terminal with this command: `ssh -Y <x500>@<cluster>.msi.umn.edu`. This will automatically place you onto a login node, which can be used to browse, view files, etc. When using a cluster to perform more advanced/computational heavy tasks, grab [an srun](slurm-params.md#srun) to enter a compute node. More information about login vs compute nodes can be found on [our Partitions page](partitions.md) 

**VS Code**

More information about how to access MSI through VSCode can be found on [our VS Code page.](vscode.md)

## Permissions and Share Access

To ensure the data and code created can be accessed by all, update your `.bashrc` with the following steps (this only needs to be done the first time you access MSI). [Read what is a .bashrc](https://www.digitalocean.com/community/tutorials/bashrc-file-in-linux) if you would like to understand more but it is essentially a file of commands that are run every time you open a new terminal.

Open your `.bashrc` file with a text editor, e.g. `emacs ~/.bashrc` or `geany ~/.bashrc`.
Set umask to 002. The umask is the default permission applied to the files you create. Permissions are how self, groups (like `faird`) and other users can be given read, write, and execute access. With 002, self and group members can be given those permissions but no one else. 
You will also need to add the path to the s3policy_bin, which you can read more about on [our s3 page](s3.md#granting-bucket-access). 
Close the file and open a new terminal to apply the changes. 

Here is a template for what your `.bashrc` should look like. You can add more as you use MSI more and determine what would be helpful. See [our Tips and Tricks page](roadblocks.md#bashrc-additions) for some potentially helpful .bashrc additions. 

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
export PATH="/projects/standard/faird/shared/code/internal/utilities/s3policy_bin/:$PATH"

# Uncomment the if statement below to enable bash completion.
# if [ -f /etc/bash_completion ]; then
#  source /etc/bash_completion
# fi

# Load modules here
# module load
```

To gain access to `faird`, ask Kim or Luci to add you to that share. This is the default share that most new people or outside collaborators are added to. This is where most of the commonly used scripts/pipelines are stored so it is important to have access to this share if you will be working with CDNI softwares.

If you are looking for access to an s3 bucket, you will need to have logged into MSI at least once. 

For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
