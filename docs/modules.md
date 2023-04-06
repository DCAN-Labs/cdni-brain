# MSI Module System (Loading Software Packages)

Read: 
* [MSI module system](https://www.msi.umn.edu/support/faq/what-software-does-msi-offer-how-do-i-access-it)

* [List of MSI software modules](https://www.msi.umn.edu/software)

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