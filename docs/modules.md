# MSI Module System (Loading Software Packages)

Read: 

* [MSI module system](https://www.msi.umn.edu/support/faq/what-software-does-msi-offer-how-do-i-access-it)

* [List of MSI software modules](https://www.msi.umn.edu/software)

## Loading Modules

MSI uses a “module” system for providing access to various software packages. Modules are loaded from the command line with `module load <module name>`.

Commonly used modules by our lab include:

* **fsl** 
* **workbench** 
* **freesurfer**
* **matlab**
* **python3**

    **	**


Often there are multiple versions of a given module available with one designated as the default to load;  `module avail <module name>` will display a list of all available versions.


**NOTE:** modules modify the user’s PATH variable while loaded, which can conflict with other modules and tools. Known conflicts include: 

* **workbench** and **freesurfer**
* **workbench** and **s3cmd**

Unloading a module with `module rm <module name>` reverts the user’s PATH variable, which should resolve the issue.

## Requesting Module from MSI

To request the inclusion of a module within MSI's infrastructure, follow the guidelines below. 

Use the provided email structure below as a template.
Replace 'X' with the actual name of the module you are requesting support for.
Specify 'Y' with a brief description of what the module will be used for on MSI.

```
Subject: Module Support Request: [Module Name] for [Description]

Hi,

I am reaching out to request support for the module X to be integrated into MSI's infrastructure for Y purpose.

The PI groups Fair, Feczko, Miranda Dominguez, Nelson, Tervo-Clemmens, Larson, and Randolph would be actively using this module.

Link to module: [Insert Module Link]

Thanks,
[Your Name]
```

Emphasize the active involvement of the PI groups (Fair, Feczko, Miranda Dominguez, Nelson, Tervo-Clemmens, Larson, and Randolph) in utilizing this module.

If available, provide a direct link to the module for easy reference by the support team.
Send it to help@msi.umn.edu.

Be prepared for possible clarifications or questions from MSI.
Respond promptly and provide any necessary additional information.