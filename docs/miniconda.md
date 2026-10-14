# Miniconda Environments (Python)

DCAN Labs maintains shared Miniconda environments for commonly used Python tools and workflows, including Dcm2Bids.

For general MSI guidance on creating and managing Conda environments, installing Python packages, and choosing appropriate installation locations, see the [MSI Conda Best Practices documentation](https://userdocs.msi.umn.edu/software/conda.html) and [Installing Python Packages](https://userdocs.msi.umn.edu/software/installing_python.html).

The sections below describe CDNI-specific shared environments and conventions.


**IMPORTANT**:  Environments and installed packages within the lab-wide environment **must** have group read and write permissions open to ensure access for all group users! Visit the [directory and file permissions section](msi-login.md#directory-and-file-permissions) for info on using a umask to set your default permissions to be group-accessible. If you need to fix the permissions of an existing environment and/or package in the lab-wide environment, those can be found at `/projects/standard/faird/shared/code/external/envs/miniconda3/mini3/envs/` and `/projects/standard/faird/shared/code/external/envs/miniconda3/mini3/pkgs/`.

## Conda Usage 

To load the DCAN labwide miniconda3 environment, first run the following command: 

        source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh 

* Create or use a dedicated environment for a specific project or workflow rather than modifying the shared base environment whenever possible.
* Activating a new Conda environment does not automatically inherit packages installed in the base environment.
* The CDNI shared Miniconda installation can be activated regardless of which MSI project share you are currently working in.

For general Conda commands such as listing, creating, activating, and inspecting environments, see the [MSI Conda Best Practices documentation](https://userdocs.msi.umn.edu/software/conda.html).

## Creating Environments

When a new CDNI-specific environment is needed, create a dedicated environment rather than modifying the shared base environment.

For current MSI guidance on Conda environment creation, package installation, channels, environment management, and Python package best practices, see:

- [MSI Conda Best Practices](https://userdocs.msi.umn.edu/software/conda.html)
- [Installing Python Packages](https://userdocs.msi.umn.edu/software/installing_python.html)

For information about using Conda environments with VS Code in CDNI workflows, see our [VS Code page](vscode.md).

If you create a new shared CDNI environment, document it in the CDNI Miniconda tracking spreadsheet described below.

## Available Environments

There are quite a few environments that exist in the lab-wide conda environment for commonly used pipelines/processes/code etc. The CDNI Google Drive has a [miniconda spreadsheet](https://docs.google.com/spreadsheets/d/1JZt_-U-ry5y0iNDujvlFfHLA4bYPlqhsif2aeeSiK2g/edit?usp=sharing) that lists all of the available environments and their purpose. If you create a new conda environment, please update this sheet! 

The shared base environment may include commonly used Python packages, but its contents can change as CDNI workflows are updated.

Before relying on a package being present, activate the environment and check the installed packages directly. If a workflow requires stable or reproducible package versions, use a dedicated Conda environment rather than depending on the shared base environment.

For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).
