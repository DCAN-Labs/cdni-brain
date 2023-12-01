# Miniconda Environments (Python)

DCAN Labs maintains lab-wide shared miniconda environments which are configured for ease of access to several of our commonly used Python tools such as [Dcm2bids](#9-dicom-to-bids-conversion-dcm2bids). Using the shared environment is in most cases preferred to individual users doing their own Python package installations and/or setting up environments in their MSI home directory.

* To load the DCAN labwide miniconda3 environment, first run the following command: 

        source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh 

* Note: It is advised to create your own environment within this miniconda3 for whatever you need miniconda3 for. That way we are not constantly changing the versioning of packages on the base environment. 
* To list the available environments within the miniconda environment:

        conda info --envs

* To activate a specific environment: 

        conda activate environment_name 

* To list the packages within an environment (if the environment is activated you do not need to specify the name):

        conda list -n environment_name

To create a new conda environment, follow these instructions:

1. Load the miniconda3 base environment - as seen above
2. Check to make sure your environment does not already exist by running `conda info --envs`
3. Run `conda create --name your_env_name`
4. Run `conda activate your_env_name`
5. If you need to install with pip, run `conda install pip` 
6. For installs on things included with miniconda3 run `conda install package_name1 package_name2`

    Note: It is a list without commas. You can also install one by one.

More information about creating and using conda environments can be found [here](https://dcan-labs-informational-guide.readthedocs.io/en/latest/vscode/#conda-environments)

** **

You can also create an environment with a YAML file of requirements.

1. Load the miniconda3 base environment - as seen above
2. Check to make sure your environment does not already exist by using `conda info --envs`
3. Run `conda env create -f /path/to/yaml/for/build.yml`
4. Run `conda activate the_env_name`

    Check out the [conda user documentation](https://docs.conda.io/projects/conda/en/stable/index.html) and [cheat sheet](https://docs.conda.io/projects/conda/en/stable/user-guide/cheatsheet.html) for further information.