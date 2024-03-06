# MSI Module System (Loading Software Packages)

Read: 

* [MSI module system](https://www.msi.umn.edu/support/faq/what-software-does-msi-offer-how-do-i-access-it)

* [List of MSI software modules](https://www.msi.umn.edu/software)

## Loading Modules

MSI uses a “module” system for providing access to various software packages. Often there are multiple versions of a given module available with one designated as the default to load;  `module avail <module name>` will display a list of all available versions. Modules are loaded from the command line with `module load <module name>`.

Commonly used modules by our lab include:

* **fsl** 
* **workbench** 
* **freesurfer**
* **matlab**
* **python3**

    **	**



**NOTE:** modules modify the user’s PATH variable while loaded, which can conflict with other modules and tools. Known conflicts include: 

* **workbench** and **freesurfer**
* **workbench** and **s3cmd**

## Removing Modules and Resolving Conflicts 

Unloading a module can be done with `module rm <module name>`. Unloading a module reverts the user’s PATH variable, which can resolve any issues relating to conflicts. 

Conda environments and user profile installs can also cause conflicts with the module system on MSI. If running `module load <module name>` doesn't successfully load the specified module, this potentially means that module is already being called from another spot. Use `which <module name>` to see where it is coming from, however, this will not work for determining the version. 

Possible ways to resolving conflicts include deactivating your current conda environment or clearing out your local user installs (if that is the specified path where from which your module is loaded). However, deactivating your current conda environment may not make sense if you need other installs within that environment for your use case.  

## Requesting a Module from MSI

To request the inclusion of a new module within MSI's infrastructure, follow the guidelines below. 

Use the provided email structure below as a template:

```
Subject: Module Support Request: [Module Name] for [Description]

Hi,

I am reaching out to request support for the module [Module Name] to be integrated into MSI's infrastructure for [brief description of module purpose on MSI]. 

The PI groups Fair, Feczko, Miranda Dominguez, Nelson, Tervo-Clemmens, Larson, and Randolph would be actively using this module. [Include any other relavant information such as examples of the work, study or project it is going to be used for.]

Link to module: [Insert Module Link]

Thanks,
[Your Name]
```

Emphasize the active involvement of the PI groups (Fair, Feczko, Miranda Dominguez, Nelson, Tervo-Clemmens, Larson, and Randolph) and the relevant work that utilizes this module. Important to list all the PI groups everytime even if some of them wont be using the module right away or at all.

If available, provide a direct link to the module for easy reference by the support team.
Send it to help@msi.umn.edu.

Be prepared for possible clarifications or questions from MSI.
Respond promptly and provide any necessary additional information.