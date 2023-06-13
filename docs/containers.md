# Containers

Read:

* [Intro to Singularity and Docker Containers @ Cornell BioHPC](https://biohpc.cornell.edu/doc/singularity_v3.pdf)
* [Singularity on CentOS7 @ MSI](https://www.msi.umn.edu/support/faq/how-do-i-use-singularity-centos-7)
* [Singularity tutorial @ MSI](https://www.msi.umn.edu/tutorials/singularity-containers)
* [Singularity quick start guide @ Syllabus](https://docs.sylabs.io/guides/latest/user-guide/quick_start.html)

The primary container format used by DCAN Labs on MSI is Singularity. Containers are used to access multiple data files on a host system, by binding those files together within a container. **Note that Singularity by default mounts your home directory into the container, meaning the container is not fully isolated.** Recommended in general to use the `--cleanenv` and `--no-home` options when running a Singularity container to avoid potential issues caused by the user’s environment or home directory.


Singularity can pull and build Docker images from Dockerhub in Singularity format (note: Docker format images cannot be run on MSI as they require root permissions.) 


Example:


`srun  -c 8 --x11 --mem=64G --tmp=200g -t 3:00:00 -p interactive --pty bash`


`module load singularity`


`singularity pull name_of_singularity_image.sif docker://path_to_docker_image`


Note: `name_of_singularity_image.sif` can be anything. Generally, it is advisable to name it with the name of the codebase + the date. Also, `dcanumn` will typically be involved in `/path_to_docker_image`


Containers are typically run with a **singularity run** command, as demonstrated in various [sbatch script examples](pipelines.md) in this handbook.


**singularity shell** launches a shell within the container, which is often useful for debugging. Will allow you to interact with the folders inside the container. 


`singularity shell name_of_singularity_image.sif`


For pulling Docker images, see [this document](https://docs.google.com/document/u/0/d/1w1g0kLSchPKvEI9pZIBmhavFd2Mq2-r82ozVaBuL9EI/edit). 


*Note: make sure MIDB’s Virtual Machine is accessible for you before pulling Docker images. [See here to go through the process](vm.md).*

## Building a Docker Container 

Before you build a container, you might need to get access to the VM at MSI where the container will be built. Find out [how to get access here](vm.md)

Once you have access, open a terminal at MSI and ssh into the VM

- Run `ssh umii-midbig-dev-docker.oit.umn.edu`

- If you are building a CABINET container, you do not need to ssh into the VM; this container is too big to be built there and can be built directly on the Linux machine

Assuming you have a github repository that has the dockerfile to build your image and any prerequisite files, download the repository to the machine

- Run `git clone html-to-repo --branch name-of-branch-to-clone`

- If you are asked for your credentials, enter your github username, then open Github and go to Settings -> Developer Settings (located at very bottom) -> Personal Access Tokens and click Generate New Token. Enter the new token ID as your password.

Navigate into the cloned repository 

- Run `cd name-of-repo`

Build the container with docker. The version (1.0. in the example command) is whatever you name it, check the docker documentation for acceptable container names. If there is an existing repository on Docker hub, include `dcanumn` before `/pipeline`. If you want to build using cache files from previous builds, do not include the `--no-cache` flag.

- Run `sudo docker build -t /pipeline-name:1.0. --no-cache`

While you are building the container, docker moves line by line through the Dockerfile and builds new images at various points in the build process. If you encounter an error during this building process, you can load the most recent intermediary image created during the build process and run the remaining portion of your Dockerfile from there. Below is an example of what the error message might look like. 

![Docker Error Example](img/docker_error_example.jpeg)

- Run `sudo docker run -it b087e2fc15b5` to load the most recent image layer (from the above example)

- If you have the line similar to the `RUN unzip ./mcr.zip`, run it without the RUN statement (`unzip ./mcr.zip`) to get a better understanding of why your command is having issues.

If the virtual machine runs out of memory while you're doing this try the following:

- Run  `docker system prune`; this is a good option if you've already removed larger files and containers

- Delete some of your old filles

- Delete specific containers by first finding their ID with `ls`, then removing them 

    - Run `sudo docker image ls` 

    - Run `sudo docker image rm alpha-numerica-container-id`

If you want to look into your container and test its behavior without running the command associated with its entrypoint, use the following command: 

- `sudo docker run -it --entrypoint /bin/bash/pipeline_name:pipeline_version`

If your container build was successful, then push it to Dockerhub (assuming there is an existing repository on Dockerhub)

- Run `sudo docker push dcanumn/container-name:container:version`

Start an interactive job on mesabi to use to build the container, requesting about twice the amount of temp space (`--tmp`) as the size of the container

- Example srun command: `srun -N 1 --ntasks-per-node=1  --tmp=100g --mem-per-cpu=30g -t 3:00:00 -p interactive --pty bash`

To build the singularity image from Docker Hub run: 

        module load singularity 
        cd /path/to/where/container/is/stored
        singularity pull name_for_singularity_image.sif docker://account/container_name:container_version

        * The account will probably be dcanumn *

If you pushed the image to Docker Hub, you do not need to also perform the following steps

If the build was successful but there is not a repository on Dockerhub for you to push to, save the build as a tar

- Run `sudo docker save pipeline-name:pipeline_version > /path/to/save/tar/image/pipeline_name_version.tar` 

Send the tar'd docker image to the main file storage system at MSI so that we can use the singularity instance installed at MSI to convert the docker image into a singularity image 

- Run `scp /path/to/tar/file/on/virtual_machine/pipeline-name.tar user_id@mesabi.msi.umn.edu:/path/at/msi/to/store/container/` 

To build the image from docker archive, grab an srun then run:

        module load singularity
        cd /path/to/where/container/is/stored
        singularity build name_for_singularity_image.sif docker-archive://pipeline-name.tar

