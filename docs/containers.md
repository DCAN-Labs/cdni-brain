# Containers

    Read:[ ](https://biohpc.cornell.edu/doc/singularity_v3.pdf)

* [Intro to Singularity and Docker Containers @ Cornell BioHPC](https://biohpc.cornell.edu/doc/singularity_v3.pdf)
* [Singularity on CentOS7 @ MSI](https://www.msi.umn.edu/support/faq/how-do-i-use-singularity-centos-7)
* [Singularity tutorial @ MSI](https://www.msi.umn.edu/tutorials/singularity-containers)
* [Singularity quick start guide @ Syllabus](https://docs.sylabs.io/guides/latest/user-guide/quick_start.html)

The primary container format used by DCAN Labs on MSI is Singularity. Containers are used to access multiple data files on a host system, by binding those files together within a container. **Note that Singularity by default mounts your home directory into the container, meaning the container is not fully isolated.** Recommended in general to use the --cleanenv and --no-home** **options when running a Singularity container to avoid potential issues caused by the user’s environment or home directory.


Singularity can pull and build Docker images from Dockerhub in Singularity format (note: Docker format images cannot be run on MSI as they require root permissions.) 


Example:


    srun  -c 8 --x11 --mem=64G --tmp=200g -t 3:00:00 -p interactive --pty bash


module load singularity


singularity pull name_of_singularity_image.sif docker://path_to_docker_image


Note: name_of_singularity_image.sif can be anything. Generally, it is advisable to name it with the name of the codebase + the date. Also, dcanumn will typically be involved in /path_to_docker_image


Containers are typically run with a **singularity run **command, as demonstrated in various sbatch script examples in this document.


**singularity shell** launches a shell within the container, which is often useful for debugging.** **Will allow you to interact with the folders inside the container. 


**	**singularity shell name_of_singularity_image.sif


For pulling Docker images, see [this document](https://docs.google.com/document/u/0/d/1w1g0kLSchPKvEI9pZIBmhavFd2Mq2-r82ozVaBuL9EI/edit). 


*Note: make sure MIDB’s Virtual Machine is accessible for you before pulling Docker images. [See here to go through the process](vm.md).*