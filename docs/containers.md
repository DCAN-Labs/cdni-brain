# Containers

Read:

* [Intro to Singularity and Docker Containers @ Cornell BioHPC](https://biohpc.cornell.edu/doc/singularity_v3.pdf)
* [Intro to Containers @ MSI](https://msi.umn.edu/getting-started/tutorials/introduction-containers-msi)
* [Official Apptainer Quick Start Guide](https://apptainer.org/docs/user/main/quick_start.html)


_The commands `singularity` and `apptainer` can be used interchangeably on MSI._
 
The primary container format used by CDNI on MSI is Singularity (aka Apptainer). Containers are used to access multiple data files on a host system, by binding those files together within a container. **Note that Singularity by default mounts your home directory into the container, meaning the container is not fully isolated.** Recommended in general to use the `--cleanenv` and `--no-home` options when running a Singularity container to avoid potential issues caused by the user’s environment or home directory.


Singularity can pull and build Docker images from Dockerhub in Singularity format (note: Docker format images cannot be run on MSI as they require root permissions.) 


Example:

`ssh -Y agate`


`srun -N 1 -n 1 -c 1 --mem=128G --tmp=200G -t 720 -p interactive --pty bash`


`module load singularity`


`singularity pull name_of_singularity_image.sif docker://path_to_docker_image`


Note: For Dockerhub images, the path to use is the path given from the Dockerhub repo "Tags" page, with `singularity pull docker://` in place of `docker pull`. e.g. `docker pull nipreps/nibabies:25.2.1`on Dockerhub becomes `singularity pull docker://nipreps/nibabies:25.2.1`


Containers are typically run with a **singularity run** command, as demonstrated in various [sbatch script examples](pipelines.md) in this handbook.


**singularity shell** launches a shell within the container, which is often useful for debugging. Will allow you to interact with the folders inside the container. 


`singularity shell name_of_singularity_image.sif`

To leave the shell, run `exit`

## Dockerhub build process

(To be updated)


## Special Container Builds 

**10.5T NHP ABCD BIDS Pipeline Synth**

**PLEASE NOTE: This does NOT apply to the regular NHP ABCD BIDS pipeline.**

This pipeline doesn't use the layered build with **internal-tools** and **external-software** like the DCAN ABCD-BIDS, NHP, Infant, pipelines. It instead uses git submodules and git lfs to link certain dependencies such as the DCAN Bold Processing repo and the macaque image templates. 

In order to build this container, you first need to clone the correct repository and initialize the git submodules. You can do this by running the following commands: 

                git clone git@github.com:DCAN-Labs/nhp-abcd-bids-pipeline-synth.git
                cd nhp-abcd-bids-pipeline-synth
                git submodule update --init

After this, you will need to run the download script in the tools subdirectory to download the templates used in the pipeline. 

- `./tools/download-templates.sh`

Once you run this, check for the existance of a *templates* folder under *scripts/dcan_macaque_pipeline/global/* 

Be sure that Docker BuildKit is enabled by adding `DOCKER_BUILDKIT=1` before your build command. An example docker build command would be

`sudo DOCKER_BUILDKIT=1 docker build . -t dcanumn/nhp-abcd-bids-pipeline-synth:[tag]`

If you need to update the link to the git submodule, for example if there are updates to the DCAN Bold Processing repo that you want to incorporate into this pipeline, this can be done by following [this example](https://stackoverflow.com/questions/1777854/how-can-i-specify-a-branch-tag-when-adding-a-git-submodule/1778247#1778247) which is noted below. 

If you want to move the submodule to a particular tag:

                cd submodule_directory
                git checkout v1.0
                cd ..
                git add submodule_directory
                git commit -m "moved submodule to v1.0"
                git push

If updating DCAN Bold Processing, the <submodule_directory> would be the path to the DCAN Bold Processing `/nhp-abcd-bids-pipeline-synth/scripts/dcan-bold-processing/`

Then, another developer who wants to have submodule_directory changed to that tag, does this

                git pull
                git submodule update --init


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).
