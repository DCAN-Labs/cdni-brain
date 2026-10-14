# MSI Module System (Loading Software Packages)

MSI uses software modules to provide access to installed software packages and different software versions.

For current instructions on finding, loading, unloading, and managing modules, see the [MSI Software Modules documentation](https://userdocs.msi.umn.edu/software/software_modules.html).

For the current list of software available on MSI systems, see the [MSI Available Software documentation](https://userdocs.msi.umn.edu/software/package_docs/index.html).

## Loading Modules

The sections below highlight modules commonly used within CDNI. For current module syntax and available versions, refer to MSI's documentation above.

Commonly used modules by our lab include:

* fsl 
* workbench
    * Some CDNI workflows may require a specific Workbench version for compatibility. Use `module avail workbench` to see the versions currently available on MSI, and load the version required by your workflow.
* freesurfer
* matlab
    * Use `module avail matlab` to see the versions currently available on MSI.

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
