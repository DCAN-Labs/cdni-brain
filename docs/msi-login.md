# Getting Started with Minnesota Supercomputing Institute (MSI)

If you are going to be processing, analyzing, or otherwise interacting with MRI data, you will need to have access to MSI. Visit  [Eligibility & Access Instructions](https://www.msi.umn.edu/content/eligibility-getting-access) for more detailed guidelines on eligibility and access requirements. This page outlines the steps needed to access MSI. 

## Duo 2-Factor Authentification

You must set up Duo 2 Factor Authentification in order to use MSI and any internal UMN site. This provides an added layer of security. UMN provides a [Duo Guide](https://it.umn.edu/services-technologies/self-help-guides/duo-set-use-duo-security) which provides instructions for how to register and use Duo.

When connecting to MSI through SSH, you will be prompted to complete Duo authentication. See the [MSI SSH Keys guide](https://userdocs.msi.umn.edu/connect/ssh_keys.html) for the current SSH authentication and connection process.

## Connecting to the UMN VPN

Access to MSI requires a connection to the UMN network. When on campus, you can connect through `eduroam` or the campus network. When working off campus, connect to the UMN VPN before accessing MSI.

For MSI-specific network requirements, see the [MSI Interactive HPC documentation](https://userdocs.msi.umn.edu/compute/interactive_compute.html). For VPN installation and connection instructions, see the [UMN VPN guide](https://it.umn.edu/services-technologies/virtual-private-network-vpn).

## Connecting to MSI

**Remote Desktop**

MSI's Open OnDemand service provides browser-based access to interactive desktops, files, terminal sessions, jobs, and applications running on MSI resources. For current instructions on accessing and using Open OnDemand, see the [MSI Open OnDemand documentation](https://userdocs.msi.umn.edu/compute/open-ondemand-support.html).

For information about interactive compute sessions and when to use Open OnDemand versus the command line, see [Interactive HPC](https://userdocs.msi.umn.edu/compute/interactive_compute.html).

**Local Terminal**

MSI can also be accessed through SSH from a terminal on your local computer. MSI maintains the current SSH configuration and key setup instructions, including directions for macOS, Linux, and Windows.

Follow the [MSI SSH Keys guide](https://userdocs.msi.umn.edu/connect/ssh_keys.html) to configure SSH access.

For information about login nodes, compute nodes, and appropriate usage of each, see [MSI Compute Need to Know](https://userdocs.msi.umn.edu/compute/cluster_info.html). For interactive command-line jobs using `srun`, see [Interactive HPC](https://userdocs.msi.umn.edu/compute/interactive_compute.html).

**VS Code**

MSI supports connecting to compute resources through Visual Studio Code using SSH. See [MSI's Connecting to Compute documentation](https://userdocs.msi.umn.edu/connect/connect_compute.html#connect-from-visual-studio-code-vscode) for MSI-specific connection guidance.

For CDNI-specific VS Code configuration and workflows, see our [VS Code page](vscode.md).

## Permissions and Share Access

To ensure that data and code created within CDNI projects can be appropriately shared with collaborators, configure your `.bashrc` as described below. This CDNI-specific configuration only needs to be completed once.

For general information about MSI project and shared storage locations, see the [MSI File Storage documentation](https://userdocs.msi.umn.edu/storage/storage.html).

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
