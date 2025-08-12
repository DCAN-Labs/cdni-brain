# MSI Module System (Loading Software Packages)

Read: 

* [MSI module system](https://www.msi.umn.edu/support/faq/what-software-does-msi-offer-how-do-i-access-it)

* [List of MSI software modules](https://www.msi.umn.edu/software)

* [Environment Modules official documentation](https://modules.readthedocs.io/en/latest/)

## Loading Modules

MSI uses the Environment Modules "module" system for providing access to various software packages. Often there are multiple versions of a given module available with one designated as the default to load.  `module avail <module name>` will display a list of all available versions. Modules are loaded from the command line with `module load <module name>/<version>`.

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

Modules modify the user’s PATH variable while loaded, which can occasionally conflict with other modules and tools. This is why it is recommened to not load too many modules by default in your `.bashrc`. Unloading a module can be done with `module rm <module name>`. Unloading reverts the PATH variable, which should resolve conflicts. Note that loading certain modules triggers the loading of additional modules as dependencies; e.g. `module load freesurfer` also loads `matlab` and `netcdf`. In these cases, unloading the first module `module rm freesurfer` does **not** unload those additional modules. 

Conda environments and user profile installs can also cause conflicts with the module system on MSI. If running `module load <module name>` doesn't successfully load the specified module, this potentially means that module is already being called from another spot. Use `which <module name>` to see where it is coming from. However, this won't always work for determining the version. 

Possible ways to resolving these conflicts include deactivating your current conda environment or clearing out your local user installs (if that is the specified path where from which your module is loaded). However, deactivating your current conda environment may not make sense if you need other installs within that environment for your use case.  

## Requesting a Module from MSI

To request the inclusion of a new module within MSI's infrastructure, follow the guidelines below. 

Use the provided email structure below as a template:

```
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
