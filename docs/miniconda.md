# Miniconda Environments (Python)

DCAN Labs maintains lab-wide shared miniconda environments which are configured for ease of access to several of our commonly used Python tools such as [Dcm2bids](dcm2bids.md). Using the shared environment is in most cases preferred to individual users doing their own Python package installations and/or setting up environments in their MSI home directory.


**IMPORTANT**:  Environments and installed packages within the lab-wide environment **must** have group read and write permissions open to ensure access for all group users! Visit the [directory and file permissions section](msi-login.md#directory-and-file-permissions) for info on using a umask to set your default permissions to be group-accessible. If you need to fix the permissions of an existing environment and/or package in the lab-wide environment, those can be found at `/projects/standard/faird/shared/code/external/envs/miniconda3/mini3/envs/` and `/projects/standard/faird/shared/code/external/envs/miniconda3/mini3/pkgs/`.

## Conda Usage 

To load the DCAN labwide miniconda3 environment, first run the following command: 

        source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh 

* Note: It is advised to create your own environment within this miniconda3 for whatever you need miniconda3 for. That way we are not constantly changing the versioning of packages on the base environment. 

* The base environment has its own set of commonly used packages installed in it. However, when you activate a new environment within the base environment, it will NOT inherit the packages installed in the base environment.

* This conda environment activates on any share. You do not have be working on faird to activate this conda environment.

To list the available environments within the miniconda environment:

     conda info --envs

To list the packages within an environment (if the environment is activated you do not need to specify the name):

     conda list -n environment_name

## Creating Environments

To create a new conda environment and managing conda environments on MSI, see [MSI Conda Best Practices](https://userdocs.msi.umn.edu/software/best-practices-conda.html).

More information about creating and using conda environments within VS code can be found on the [VSCode page](vscode.md#conda-environments)

Check out the [Conda user documentation](https://docs.conda.io/projects/conda/en/stable/index.html) and [cheat sheet](https://docs.conda.io/projects/conda/en/stable/user-guide/cheatsheet.html) for further information.

## Available Environments

There are quite a few environments that exist in the lab-wide conda environment for commonly used pipelines/processes/code etc. The CDNI Google Drive has a [miniconda spreadsheet](https://docs.google.com/spreadsheets/d/1JZt_-U-ry5y0iNDujvlFfHLA4bYPlqhsif2aeeSiK2g/edit?usp=sharing) that lists all of the available environments and their purpose. If you create a new conda environment, please update this sheet! 

The base environment also includes some commonly used packages that can be helpful for running code that imports special libraries. These include `bids-validator`, `dcm2bids`, `dcm2niix`, `debugpy`, `h5py`, `matplotlib`, `mkdocs`, `nipype`, `nibabel`, `numpy`, `pandas`, `pybids`, `pyyaml`, `scipy`, `seaborn`, and more! If you have a script that requires a special library, try activating the base environment to see if that package is in there. 

For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).
