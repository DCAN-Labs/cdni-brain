# MSI Module System (Loading Software Packages)

MSI uses software modules to provide access to software packages and different software versions without requiring users to install them individually.

For current instructions on finding, loading, unloading, and managing software modules, see the [MSI Software Modules documentation](https://userdocs.msi.umn.edu/software/software_modules.html).

For the current software available on MSI systems, see the [MSI Available Software documentation](https://userdocs.msi.umn.edu/software/).

Commonly used modules by our lab include:

* fsl 
* workbench 
    - Note that the default version used to be 1.5.0 but is now 2.0.1, which has different default settings. Depending on use case you may need to load workbench/1.5.0, 1.4.2, or another older version for compatability
* freesurfer
* matlab (most versions from R2010b through R2023b available)

Some other helpful modules include:

* tree
    - Allows you to print a directory structure tree

* libreoffice
    - Helpful for viewing csv files (similar to excel). Note that you must provide the full path to the file you wish to view, a relative path won't work

* vscode
    - Allows you to use VS Code on MSI

* cubids
    - Alternative option instead of loading the cubids miniconda environment

* singularity
    - For building/accessing singularity (/ Apptainer) images 

## Removing Modules and Resolving Conflicts

Loaded modules can modify environment variables such as `PATH`, which can occasionally cause conflicts between software packages or user-installed tools.

For current instructions on viewing, unloading, and clearing modules, see the [MSI Software Modules documentation](https://userdocs.msi.umn.edu/software/software_modules.html).

Useful commands include:

```bash
module list
module unload <package-name>
module purge
module show <package-name>
```

## Requesting a Module from MSI

To request the inclusion of a new module within MSI's infrastructure, follow the guidelines below. 

Use the provided email structure below as a template:

Subject: Module Support Request: [Module Name] for [Description]

Hi,

I am reaching out to request support for the module [Module Name] to be integrated into MSI's 
infrastructure for [brief description of module purpose on MSI]. 

The PI groups faird, feczk001, miran045, smnelson, btervocl, bart, and rando149 would 
be actively using this module. [Include any other relavant information such as examples 
of the work, study or project it is going to be used for.]

Link to module: [Insert Module Link]

Thanks,
[Your Name]
```

Emphasize the active involvement of all the [PI groups](hcp.md#tier-1-storage) and the relevant work that utilizes this module. It is important to list all the PI groups everytime even if some of them wont be using the module right away or at all.

If available, provide a direct link to the module for easy reference by the support team.
Send it to help@msi.umn.edu.

Be prepared for possible clarifications or questions from MSI.
Respond promptly and provide any necessary additional information.


For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
