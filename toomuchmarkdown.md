<!-----

You have some errors, warnings, or alerts. If you are using reckless mode, turn it off to see inline alerts.
* ERRORs: 0
* WARNINGs: 0
* ALERTS: 2

Conversion time: 6.869 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β34
* Fri Feb 10 2023 12:10:19 GMT-0800 (PST)
* Source doc: Data Processing Handbook
* Tables are currently converted to HTML tables.
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server. NOTE: Images in exported zip file from Google Docs may not appear in  the same order as they do in your doc. Please check the images!

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 2.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>



## **_Table of Contents_**


[TOC]



## **_Overview_** {#overview}

This handbook outlines key data processing tools and resources used by DCAN Labs. Topics covered include MSI basics and the Slurm job scheduler, MRI data conversion and initial quality control, data processing pipelines, post-processing quality control, and best practices for data storage and transfer.



1. Slurm

**  	**Read:



* [Slurm @ MSI documentation ](https://www.msi.umn.edu/slurm)
* [Slurm official site](https://slurm.schedmd.com/documentation.html)
* [Slurm commands cheat sheet](https://slurm.schedmd.com/pdfs/summary.pdf)

    **Slurm** is MSI’s job scheduling system. It is responsible for managing the allocation of computing resources among MSI users and user groups. In the DCAN labs, most data processing tasks are executed via Slurm job submissions, in the form of **batch scripts** submitted with the **sbatch **command. A batch script is a text file that specifies both the resources (e.g. CPUs, RAM, time) and the commands to be run for a processing job.


    Slurm also has “interactive” jobs, which allow access to compute resources directly from the terminal, as opposed to batch job submissions which run in the background.


        **_NOTE: Login nodes, e.g. “ah1001”, “ln1001” should only be used for minimal tasks like batch script submission, filesystem browsing, small file transfers, and text editing.  Jobs requiring more time or resources should be run via sbatch submission, srun, or interactive session with dedicated compute resources i.e. NICE. Processes on login nodes are automatically terminated after 15 minutes. _** 


    Also available are various commands for job accounting, job management, and environment configuration. (See cheat sheet linked below)


    **A table summarizing some commands that can be used inside Slurm job scripts ([sruns](#4-1-srun-immediately-run-a-command-using-the-specified-compute-resources) and [sbatchs](#4-2-sbatch-copies-the-script-in-an-internal-storage-and-then-uploads-it-on-the-compute-node-when-the-job-starts))**. The first four commands are required, while the other commands are optional. See [here](https://slurm.schedmd.com/sbatch.html) for a larger list of options. For parameter optimization, [refer to using seff in this section](#15-optimizing-sbatch-parameters).


<table>
  <tr>
   <td>
<strong>Slurm command</strong>
   </td>
   <td><strong>Effect</strong>
   </td>
  </tr>
  <tr>
   <td>#!/bin/bash -l 
   </td>
   <td><em>Required for sbatch.</em> Specifies how the Slurm file should be read (by the bash interpreter). A statement like this is required to be the first time of a Slurm script
   </td>
  </tr>
  <tr>
   <td>-t 8:00:00
<p>
--time=8:00:00
   </td>
   <td><em>Required.</em> Specifies the maximum limit for how long the job will be allowed to run 
   </td>
  </tr>
  <tr>
   <td>--ntasks=8
   </td>
   <td>Required: Specifies the number of processors (cores) that will be reserved for the job
   </td>
  </tr>
  <tr>
   <td>-A, --account=<em>share</em>
   </td>
   <td>Optional, but highly recommended: Charge resources used by this job to the specified account or default account. The account name may be changed after job submission using the scontrol command. To choose an optimal share, see the Fairshare explanation <a href="#5-fairshare">here</a>.
   </td>
  </tr>
  <tr>
   <td>-c 3
<p>
--cpus-per-task=3
   </td>
   <td>Optional: Without this option, the controller will just try to allocate one processor per task. For instance, consider an application that has 4 tasks, each requiring 3 processors. If our cluster is comprised of quad-processor nodes and we simply ask for 12 processors, the controller might give us only 3 nodes. However, by using the --cpus-per-task=3 options, the controller knows that each task requires 3 processors on the same node, and the controller will grant an allocation of 4 nodes, one for each of the 4 tasks.
   </td>
  </tr>
  <tr>
   <td>--mem=10g
   </td>
   <td>Optional: Specifies the maximum limit for memory usage for the entire job. This job will die if the application tries to use more than 10GB of memory*
   </td>
  </tr>
  <tr>
   <td>--mem-per-cpu=<em>&lt;size>[units]</em>
   </td>
   <td>Optional: Minimum memory required per allocated CPU
   </td>
  </tr>
  <tr>
   <td>--tmp=10g
   </td>
   <td>Optional: Specifies 10GB of temporary disk space will be available for this job in /tmp. Should only be used if you are specifying /tmp folders for inputs, outputs, or working directories*
   </td>
  </tr>
  <tr>
   <td>--mail-type=ALL
   </td>
   <td>Optional: Specifies which events will trigger an email message. Other options here include NONE, BEGIN, END, and FAIL. Not recommended for 100+ subject jobs
   </td>
  </tr>
  <tr>
   <td>--mail-user=x500@umn.edu
   </td>
   <td>Optional: Specifies the email address that should be used when the Slurm system sends message emails. Make sure to double check each time. People frequently get emails from others due to other people copying and running their sbatches
   </td>
  </tr>
  <tr>
   <td>-p small,mygroup 
<p>
--partition=small,mygroup 
   </td>
   <td>Optional: Specifies the partition to be the “small” partition, or a partition named “mygroup”. The job will start at the earliest time one of these partitions can accommodate the job. You must be logged into the correct <a href="#2-interactive-high-performance-computing-hpc-resources">HPC resource</a> to access corresponding partitions. For more info, see <a href="#3-partitions">here</a>.
   </td>
  </tr>
  <tr>
   <td>--gres=gpu:v100:2
<p>
-p v100
   </td>
   <td>Optional: Request two v100 GPUs for a job submitted to the V100 group
   </td>
  </tr>
</table>



        *--mem is the amount of RAM (random access memory) on a CPU (central processing unit), while --tmp indicates the amount of temporary storage that you can utilize for a job. With whatever storage amount is specified for --tmp, that amount will be created for you within the /tmp folder to output your processing derivatives. 

**Other useful commands:**



    1. View all jobs submitted by a given user:  squeue -u username (one can also use squeue -al --me when trying to determine specific jobs for their own account) or squeue -A group_name to check the queue for all groups one is under to determine which to submit under 
    2. Cancel a submitted job: scancel jobID_number
        1. scancel can also be used for job arrays by listing the job ID numbers as a comma separated list.
    3. Display accounting data for all jobs and job steps in the Slurm job accounting log or Slurm database: sacct 
        2. See the below command to check the status of a job even after it has exited. JOBID_ARRAY can also just be the JOBID, if you didn't submit an array

            sacct -X -j JOBID_ARRAY# -o JobID,NNodes,State,ExitCode,DerivedExitCode,Comment

        3. Check the jobs you have in queue by using squeue -al --me
    4. Change job account (each PI in the lab has their own Slurm group account with its own allocation of resources and queue priority. It is sometimes useful to change accounts to distribute resource requests for large processing jobs, or when an account has low queue priority due to heavy usage) : scontrol update JobId=#### Account=new_group
        4. An example command has _Job 234293_ originally submitted under default account _miran045_, and to change it to _feczk001 _one would use: scontrol update JobId=234293 Account=feczk001
    5. To change a slurm job partition use: scontrol update JobId=#### Partition=new_partition 
        5. An example command has _Job 234293_ originally submitted with the partition _msismall_, and to change it to _msigpu _one would use: scontrol update JobId=234293 Partition=msigpu
    6. To change the amount of time a slurm job runs for, use: scontrol update JobId=#### EndTime=HH:MM:SS 
        6. To find time information, first use scontrol show JobId=####
        7. An example command has _Job 234293_ originally submitted at the following time for 96 hours: _StartTime=2022-08-29T13:04:45_, and to change it to 48 hours one would use: scontrol update JobId=234293 EndTime=2022-08-31T13:04:45
2. Interactive High Performance Computing (HPC) Resources 

    Read: [Interactive HPC @ MSI](https://www.msi.umn.edu/content/interactive-hpc)


    MSI offers several options to remotely access HPC resources

* **NICE:** Connects a user to a graphical environment, which may have specialized GPUs for visualizing large datasets. 
    * Best for data visualization
    * Terminals and folders will close when the time limit is reached on the interactive session
    * Limited CPU / RAM allocation (2 cores / 16 GB max) makes it unsuitable for more intensive tasks such as preprocessing pipelines.
* **NX NoMachine:** Connects a user to a graphical environment, which may be used as a lightweight remote desktop for connecting to other MSI resources.
    * User’s session persists even if the browser or client is closed, and upon reconnection can be resumed from where it was left (session will still be terminated by MSI maintenance day 1st Wednesday of each month, or by other downtime).
    * Great to leave terminals and folders open for weeks
* **Jupyter Notebooks:** A web portal for notebook-based computing in the browser, which can be used for reproducible and shareable data analysis, visualization, and scripted control of larger tasks. Currently supports Python 2, Python 3, and R.
    * Great for playing around with a database initially
    * Formalizing code here is not recommended
* **Terminal: **User input might come through a command line interface (i.e., system shell) while debugging a piece of software 
    * Great for working with codebases on VScode
* **srun:**  Submit an interactive job on the current HPC system (NICE, NX, or Terminal) to provide a command line or graphical interface to interactively run software.
    * Needed for running programs / visualizing data for more than 15 minutes on any system listed above
3. Partitions 	

    Read:  [Partitions @ MSI](https://www.msi.umn.edu/partitions)


    Each partition you may choose is specific to the HPC resource that is being used. 


    In the table linked above, **“Partition name”** specifies the string for the partition. Note that some partitions are** **only accessible if connected to a corresponding login node, e.g. **ahXXXX** for Agate. “Federated” partitions such **msismall** can be used from any login node, and automatically delegate jobs to a partition accessible via the current login node. 


    **“Node sharing”** specifies whether a partition allows for multiple jobs to be allocated on the same node across resources.


    ** “Cores per node”** specifies the range of core processors that may be allocated for one job per node. 


    **“Walltime limit”** specifies the maximum amount of time that is allocated for a job to use resources. 


    **“Total node memory”** is the range of memory allocated for each node. 


    **“Advised memory per core”** is effectively the amount of RAM allocated to a given CPU. 


    **“Local scratch per node”** is the total amount of --tmp storage that you can specify for that partition. 


    Finally, **“Maximum nodes per job”** is the highest number of nodes one may be allocated for each job.

4. sbatch/srun Parameters 

### 
    4.1 srun: immediately run a command using the specified compute resources {#4-1-srun-immediately-run-a-command-using-the-specified-compute-resources}


	Read: [Interactive queue use with srun @ MSI](https://www.msi.umn.edu/content/interactive-queue-use-srun)



1. **srun** is primarily useful for jobs that will take longer than 10-15 minutes to run on a login node; 15 minutes is the maximum runtime on the login node (the login node is the node you are on after logging into mesabi/agate/mangi). However, they are also very useful for loading a Matlab session (or something similar) with sufficient resources. 
2. This will allow you to work directly in a terminal, allowing you to run (resource intensive) software on it; you get the results in your terminal and you cannot write other commands until it is finished. (If you append the “&” symbol to the srun command it will execute in the background, allowing continued use of the current terminal.)
    1. Results that are displayed in the terminal will typically be the steps outputted when opening an application. Terminal outputs from an interactive session are known as stdout and stderr files for an [sbatch](#4-2-sbatch-copies-the-script-in-an-internal-storage-and-then-uploads-it-on-the-compute-node-when-the-job-starts) job.
3. If you disconnect, you will lose control over srun jobs, or they might be killed (depending on whether they use stdout or not). They will also be killed if the machine to which you connect to submit jobs is rebooted. NICE or NX are recommended to prevent this, as the virtual desktops save your working environment. 
4. Ex: srun --time=8:00:00 --mem-per-cpu=8GB --cpus-per-task=4  -A feczk001 --tmp=20gb  -p interactive --x11 --pty bash
    2. This interactive job is grabbing 8 hours on 4 cpus on the interactive partition with 8 gigabytes of memory per cpu, 20gb of temporary storage total, x11 enabled, and the ability to use your terminal, utilizing the feczk001 account’s allocated resources.
    3. Use groupquota to check which account you are currently using. If the account you specified in the interactive job does not match the account listed when you run groupquota, then use sg {share} to switch to the specified account. Then use groupquota again to make sure you are now on the correct share.
    4. x11 forwarding (--x11) enables X11 graphical apps to render correctly via remote connection.

### 
    4.2 sbatch: copies the script in an internal storage and then uploads it on the compute node when the job starts {#4-2-sbatch-copies-the-script-in-an-internal-storage-and-then-uploads-it-on-the-compute-node-when-the-job-starts}


	Read: [Job Submission & Scheduling @ MSI](https://www.msi.umn.edu/content/job-submission-and-scheduling-slurm)


    The job script needs to stay the same until the job starts to produce the correct results. If you make changes to a script before the job starts, those edits will be reflected in the job. 



1. Results are written out as your script specifies. stdout and stderr will be outputted if the -o and -e flags are specified, and you can submit other commands right away.
2. An sbatch job is handled by Slurm; you can disconnect (not run interactively), kill your terminal, etc. with no consequence
3. To run an sbatch, use this command: sbatch scriptname.sh
    1. scriptname -> the name of the job script 
    2. Example sbatch layout: 
    3. The above example uses #!/bin/bash -l to clarify that this is a bash script.
    4. The name of the job is “_abcd-xcp_”, which is a descriptive name that will appear when running squeue -al --me in the command line. 
    5. It will send all types of mail to the specified email address “lundq163@umn.edu”
    6. The resources utilized by this sbatch job include:
        1. a maximum time of 8 hours 
        2. 8 cpus per task 
        3. queueing on the msismall partition (mangi/mesabi)
        4. 160 gigabytes of total RAM 
        5. utilizing the account _rando149_’s queue
    7. There are full paths to a directory that will hold both stdout and stderr files, which are listed above as output_logs/xcp_full_%A.out and output_logs/xcp_full_%A.err. The %A signifies that the job ID number will be added to the filename.
        6. Note: make sure that the _output_logs_ directory exists prior to job submission, if you choose to use one. You can also change that path or not have a sub folder altogether if you want. It is recommended to specify the full path.
    8. All of the text after the #SBATCH parameters are the commands needed to run an XCP-D [singularity container](#21-containers). This is one example of what can be in an sbatch, and it can be substituted for any command that may need to be run via an sbatch. 
        7. The input and output directory paths are bound to the container’s filesystem using the “-B” syntax. There is also a path to a .sif file that will utilize the jobs resources in order to produce the desired outputs using what's in the input directory. This .sif file is a[ singularity image ](#21-containers)that is being run on the specified input files. The input path is described above as /home/faird/shared/projects/teaching/practicals/experiments/dcm2bids_output2/derivatives/nibabies/. The output path is /home/faird/shared/projects/teaching/practicals/experiments/dcm2bids_output2/derivatives/XCPD/ and the path to the singularity image is /home/faird/shared/code/external/pipelines/ABCD-XCP/xcp-d_unstable04152022a.sif  
            1. Note: these input, output and singularity image paths need to exist prior to running the sbatch. 
        8. The pipeline specific flags --cifti -m --participant-label and -w.
            2. --cifti will postprocess cifti files instead of niftis
            3. -m will combine all the runs into one file
            4. --participant-label is a space delimited list of participant identifiers or a single identifier
            5. -w is the path to where intermediate results should be stored. In the above sbatch, it is specified in the line that reads -B /home/feczk001/shared/projects/Jacob_test/work/:/work \ and is called on again later in this line -w /work \
5. Fairshare

    Read: [HPC Fairshare scheduling @ MSI](https://www.msi.umn.edu/content/hpc)


    The goal is to increase the priority when scheduling jobs. When a group has recently used a large amount of resources, the priorities of their waiting jobs will be negatively affected until their usage decreases.


    sshare -U x500: checks all of the accounts a user has access to at once

    7. Below is an example of a terminal output after running the above command:
    8. Based on the values in the FairShare column, the larger numbers will receive higher priority when running jobs. So with the above output, the user would want to submit jobs using feczk001.

    For checking the fairshare of a specific account, use sshare --account=$account_name -a: $account_name is the group account that you want to check the fairshare of. The group account names are: **miran045**, **faird**, **feczk001**, **rando149**. 

    9. The number within the FairShare column is a fraction of the total fairshare remaining, with larger numbers meaning a higher priority and smaller numbers meaning a lower priority 
    10. For more detail, [watch this video](https://www.youtube.com/watch?v=uKeMHDo1BkM).
6. MSI Module System (Loading Software Packages)

Read: [MSI module system](https://www.msi.umn.edu/support/faq/what-software-does-msi-offer-how-do-i-access-it)


    Read: [List of MSI software modules](https://www.msi.umn.edu/software)


    MSI uses a “module” system for providing access to various software packages. Modules are loaded from the command line with module load &lt;module name>.** **


    Commonly used modules by our lab include:



* **fsl** 
* **workbench** 
* **freesurfer**
* **matlab**
* **python3**

    **	**


    Often there are multiple versions of a given module available with one designated as the default to load;  module avail &lt;module name> will display a list of all available versions.


    **NOTE:** modules modify the user’s PATH variable while loaded, which can conflict with other modules and tools. Known conflicts include: 

* **workbench_ _**and **freesurfer**
* **workbench** and_ **s3cmd **_

    Unloading a module with module rm &lt;module name>** **reverts the user’s PATH variable, which should resolve the issue.

7. Miniconda Environments (Python)

    DCAN Labs maintains lab-wide shared miniconda environments which are configured for ease of access to several of our commonly used Python tools such as [Dcm2bids](#9-dicom-to-bids-conversion-dcm2bids). Using the shared environment is in most cases preferred to individual users doing their own Python package installations and/or setting up environments in their MSI home directory.

* To load the DCAN labwide miniconda3 environment, first run the following command: 

            source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh 

* Note: It is advised to create your own environment within this miniconda3 for whatever you need miniconda3 for. That way we are not constantly changing the versioning of packages on the base environment. 
* To activate a specific environment: 

        conda info --envs


        conda activate environment_name 


    To create a new conda environment, follow these instructions:

1. Load the miniconda3 base environment - as seen above
2. Check to make sure your environment does not already exist by running conda info --envs
3. Run conda create --name your_env_name
4. Run conda activate your_env_name
5. If you need to install with pip, run conda install pip 
6. For installs on things included with miniconda3 run conda install package_name1 package_name2

    Note: It is a list without commas. You can also install one by one.


    You can also create an environment with a YAML file of requirements.

1. Load the miniconda3 base environment - as seen above
2. Check to make sure your environment does not already exist by using conda info --envs
3. Run conda env create -f /path/to/yaml/for/build.yml
4. Run conda activate the_env_name

    Also, check out the [conda user documentation](https://docs.conda.io/projects/conda/en/stable/index.html) and [cheat sheet](https://docs.conda.io/projects/conda/en/stable/user-guide/cheatsheet.html) for further information.

8. Virtual Machine (Container Building)

    DCAN Labs has access to a Virtual Machine (VM) for the purpose of building Docker application containers (Docker builds require root access, which is not permitted to general users on the main MSI clusters, thus the need for the VM). The process to sign up for access to MIDB's VM for use with container developing is as follows:

1. Reach out to Tim, hendr522@umn.edu that you would like access to the container and indicate what project or projects that you will need it for
2. Read through the [documentation ](https://docs.google.com/document/d/1w1g0kLSchPKvEI9pZIBmhavFd2Mq2-r82ozVaBuL9EI/edit)on how to use the VM appropriately put together by Audrey 
3. Tim will submit a ticket to OIT on your behalf. Once you hear back from Tim that you do have access try to attempt the following test below to ensure that your access is working
    1. Run ssh &lt;x500>@umii-midbig-dev-docker.oit.umn.edu
    2. When prompted enter your UMN ID password and two-factor authenticate with DUO
    3. If that all works try testing if you have "sudo" privileges by typing sudo docker images
    4. If you get an output that looks like the attached picture, you are all set, if not let Tim know.
9. DICOM to BIDS Conversion (Dcm2bids)

	Read: [Dcm2bids documentation page](https://unfmontreal.github.io/Dcm2Bids/docs/get-started/)

	Watch: [Dcm2bids DCAN tutorial recording](https://drive.google.com/drive/folders/1OgyqFfpqp3qWg4OJzY9ADTwV26j8fJYf) 


    	


    Dcm2bids is a tool to convert data from DICOM format into proper [brain imaging data structure (BIDS)](https://bids-specification.readthedocs.io/en/stable/)


    Dcm2bids can be loaded via the labwide Miniconda’s **dcm2bids** environment. (See **(b)** further down in this section.) 



    11. dcm2bids requires a configuration file specifically tailored to the contents of the JSON “sidecar” files that contain metadata for the image files. The config file is a way to uniquely identify each file that needs to be converted.
        8. Example section: 
        9. Above is an example of a description field, which describes an acquisition, in this case a T2-weighted image. 
        10. The “dataType” field is mandatory for any acquisition. It is a functional group that will define different types of data: func, dwi, fmap, anat, meg, and beh.
        11. The “modalityLabel” is also mandatory. It describes the modality of the acquisition, such as T1w, T2w, dwi, or bold.
        12. The “criteria” section is used to match unique identifiers in the sidecar files with values entered in the section. “SeriesDescription” will find any file that contains the specified value. In this example, “SeriesDescription” is looking for any file that contains the characters T2 somewhere in the file name. It is also looking for “EchoTime” fields with a value of 0.1.
        13. The “sidecarChanges” field is optional and it will change or add any information in the sidecar file. In this example, the field “ProtocolName” with the value of T2 is being adjusted in the sidecar.
    12. Commands used to convert files from DICOMs to NIfTIs:
        14.  source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh
        15. conda activate dcm2bids
            1. For multi-echo data, or data acquired on a scanner with the XA30 software package, instead use conda activate dcm2bids_xa30_test
        16. dcm2bids_helper -d /path/to/input/dir/ -o /path/to/output/dir/
            2. The input directory ( -d ) contains the original dicom files.
            3. The -o flag is optional. It will create a directory for the output either in your input directory or in the output directory you specify.
            4. The dcm2bids_ helper will give you an example of the sidecars. You can use the helper with the dicoms of one participant. It will utilize dcm2niix and save the result inside the tmp_dcm2bids/helper of the output directory. 
        17. After completing your config file, to run dcm2bids, first cd into your BIDS directory. Then use this command: dcm2bids -d /path/to/input/dir/ -p participant_id -c /path/to/config_file.json
    13. For ABCC data, abcd-dicom2bids: used for selectively downloading ABCD Study imaging DICOM data QC'ed as good by the ABCD DAIC site (fast track qc), converting it to BIDS standard input data (see [here](https://www.google.com/url?q=https://collection3165.readthedocs.io/en/stable/recommendations/%233-the-bids-quality-control-file&sa=D&source=docs&ust=1660838354455769&usg=AOvVaw1E2rTHf_kzVv2xtxsQYDlw)), selecting the best pair of spin echo field maps, and correcting the sidecar JSON files to meet the BIDS Validator specification. abcd-dicom2bids still uses dicom2bids, but uses a config file that is specific to ABCC. See more information on the [repository](https://www.google.com/url?q=https://github.com/DCAN-Labs/abcd-dicom2bids&sa=D&source=docs&ust=1660838354453985&usg=AOvVaw2qzX3n8wO5qBFYaDgbMIMn). For information on how to use the fast track qc, navigate to[ the section](#24-fast-track-qc-comparison-abcd). It is important to note that when running abcd-dicom2bids , cache can fill up in your home directory under /home/{share}/x500/x500/ , so check up on and delete files within this directory when necessary. 
        18. List of commands to run abcd-dicom2bids interactively:

            module load singularity


            . "/home/faird/shared/code/external/envs/miniconda3/mini3/etc/profile.d/conda.sh"


            export PATH="/home/faird/shared/code/external/envs/miniconda3/mini3/bin:$PATH"


            export PATH=$PATH:/home/feczk001/shared/code/external/utilities/dcmtk-3.6.5/dcmtk-3.6.5-build/bin


            export PATH=$PATH:/home/feczk001/shared/code/external/utilities/jq-1.5


            python3 /home/rando149/shared/code/internal/utilities/abcd-dicom2bids_nda/abcd2bids.py /panfs/roc/msisoft/fsl/6.0.4/ /home/feczk001/shared/code/external/utilities/MATLAB_MCR/v91 -q /example/path/to/abcd_fastqc01.txt -l /example/path/to/a/subject_list_file -o /example/path/to/an/output_directory/ -p package_ID --downloadcmd /home/faird/shared/code/external/envs/miniconda3/mini3/envs/abcd-dicom2bids/bin/downloadcmd --singularity /home/rando149/shared/code/internal/utilities/abcd-dicom2bids_nda/validator_latest.sif 

        19. Example run command for a list of subjects, with good sbatch specs:
        20. In the run command, -c is the usage for creating the relevant config file, -l is the list of subjects being converted, -o is where the outputs are being stored, --temp is the path to the directory where temp files will be stored, and  --download will specify where the ABCD data will be downloaded.
        21. For more information on arguments, see the ReadMe section on Optional Arguments [here](https://github.com/DCAN-Labs/abcd-dicom2bids).
        22. For running multiple subjects, it is advised to use a [wrapper ](#17-s3-pipeline-wrappers)to submit one subject/session at a time. There is a wrapper for tier 1 here: /home/faird/shared/code/internal/utilities/slurm_pipeline_wrappers/slurm_abcd-hcp-pipeline_scripts and a wrapper for tier 2 here: /home/faird/shared/code/internal/utilities/slurm_pipeline_wrappers/slurm_abcd-hcp-pipeline_scripts_with_s3_routines_with_ses
            5. Note: if you do not have access to the faird share, please email [hough129@umn.edu](mailto:hough129@umn.edu) and they will provide you with the codebase.

   



10. NORDIC denoising and BIDS NORDIC wrapper

    NORDIC (**NO**ise **R**eduction with **DI**stribution **C**orrected) is a denoising method that can be applied to functional data. It requires that both magnitude and phase data NIfTIs exist for each run to be denoised.


    A Matlab script to apply NORDIC is located at /home/feczk001/shared/code/external/utilities/NORDIC/20211206/NIFTI_NORDIC.m


    The minimum required inputs to the script are the magnitude and phase files, plus a filename for the denoised output. 


    Additionally, the number of noise volumes at the end of the run should be specified with the ARG.noise_volume_last argument. Noise volumes, if present, should be removed from the output with fslroi (module load fsl; fslroi &lt;input> &lt;output> &lt;tmin> &lt;tsize>). “tmin” will be 0, and “tsize” should be the number of volumes in the timeseries **excluding** noise volumes at the end. 


    **Alternatively**, there is a BIDS NORDIC wrapper at /home/faird/shared/code/internal/utilities/bids_nordic_wrapper which takes a BIDS func directory (and number of noise volumes per scan) as input, runs the NORDIC script on the magnitude-phase file pairs, and includes a cleanup script to remove intermediate files created in the process. The wrapper expects a specific naming convention for the magnitude and phase timeseries; described in (1) below. From the README:


        _Usage_


        _1) Convert source data to BIDS with Dcm2bids. BIDS task names for the converted files are expected to be as follows:_


        _"task-restSE" and "task-restSEph", respectively for single-echo magnitude and phase BOLD timeseries_


        _"task-restME" and "task-restMEph" for multi-echo_


        _(An example dcm2bids config file which outputs the expected task names is in "dcm2bids_conf_examples")_


        _2) Run bids_nordic_wrapper.sh (arg1= path to func dir, arg2= number of noise volumes at end of each run)_


        _3) [Optional] Run nordic_cleanup.sh to remove logs and other files not used in further processing_

11. CuBIDS (Curation of BIDS)

    Used to make sure the data is BIDS valid (utilizing the [bids-validator package 1.7.2](https://cubids.readthedocs.io/en/latest/installation.html#:~:text=Now%20that%20we,%24), which will already be installed on MSI) and to ensure all of the acquisition parameters of the data are what you expect them to be. If you are running CuBIDS on data that has more than ten subjects, then use an [srun ](#4-1-srun-immediately-run-a-command-using-the-specified-compute-resources)or an [sbatch](#4-2-sbatch-copies-the-script-in-an-internal-storage-and-then-uploads-it-on-the-compute-node-when-the-job-starts).

    14. Load CuBIDS environment: 
        23. Load lab-wide miniconda3 environment: 

            source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

            6. This ensures that you have all of the proper packages installed to run cuBIDS.
        24. Activate cuBIDS environment

            conda activate cubids

        25. If this method did not work, see CuBIDS documentation [here](https://cubids.readthedocs.io/en/latest/installation.html).
    15. Run CuBIDS on your dataset:
        26. Validation: cubids-validate --sequential /bids/directory/path/ /output/path/with-prefix-at-the-end
            7. To determine if your file-naming and directory structure is proper BIDS
            8. To determine if any BIDS required files or metadata fields are missing.
            9. The sequential flag treats all subjects as its own BIDS dataset. You won’t see errors about subjects having differing sessions or parameters, which can be good or bad depending on what the data is.
            10. Note: the /bids/directory/path/ is the full path to the BIDS data set directory containing your subjects. The /output/path/with-prefix-at-the-end is the full path to the CuBIDS validate output file, which will have a prefix at the end. The recommended prefix to use is the date and the study as CuBIDS validate may need to be run many times.
        27. Group: cubids-group /bids/directory/path/ /output/path/with-prefix-at-the-end
            11. To group files according to acquisition, and therefore, confirming the data was acquired how you expected.
            12. To make sure all fieldmaps have IntendedFors and that all bold runs have fieldmaps paired with them for distortion correction.
            13. Note: the /bids/directory/path/ is the full path to the BIDS data set directory containing your subjects. The /output/path/with-prefix-at-the-end is the full path to the CuBIDS group output file, which will have a prefix at the end. The recommended prefix to use is the date and the study as CuBIDS group may need to be run many times.
        28. add-nifti-info can be used in the same manner as group to add information contained in the niFti file headers to the sidecar JSONs. add-nifti-info won't duplicate nifti header information in sidecar files if that information already exists.
        29. Note: /output/path/with-prefix-at-the-end can be any path that already exists that you want CuBIDS to output it’s findings to. You need to make sure this directory exists prior to running CuBIDS. You can also specify any prefix you want to identify that specific run of cubids-validate or cubids-group. Ex: 2022-10-04 (CuBIDS automatically adds a separating character.
        30. For more information on CuBIDs and running it, see the documentation [here](https://cubids.readthedocs.io/en/latest/index.html).
12. Additional BIDS Validation 
1. Process a subject with the chosen pipeline to see if it fails right away. 
    1. This will tell you if there are any “flaws” in your data that would cause the specific pipeline you are trying to run to fail. Examples:
        1. A field is missing in your sidecar JSONs
        2. The pipeline you are running requires something specific
2. Use bash or python commands to modify the data if it ends up not being BIDS compatible and/or the pipeline you are running is unable to run successfully.
        31. Examples: rename, cp, mv
            14. For how to use, insert_command --help or man insert_command 
        32. CuBIDS or the pipeline .err / .out files will let you know what is needed to fix – the degree of BIDS-valid is determined by the pipeline.
        33. Common errors: 
            15. IntendedFor sidecar JSON fields missing
            16. BIDS flags out of order in the file names
            17. Missing parameters in sidecar jsons
            18. Subject session directories have a different naming scheme than the files
        34. For more information on the BIDS specification, see this [link](https://github.com/PennLINC/CuBIDS)
13. Storage Allocation Estimates

    Figure out the approximate average storage impact your jobs (inputs and outputs) will have and estimate the total storage impact. For processing and analyzing a sample of individual subjects, run 3-5 different subjects’ jobs on Tier 1 and take the largest output and multiply it by the total number of subjects: du -sh --total /subject/output/path

    16. Make the decision of where the processing will take place
        35. **S3 storage should be used whenever you can.** Data is organized by your personal bucket, and you must manage who has permissions and access to your S3 bucket. Every user is allocated 120TB, so space is less of a concern for Tier 2 storage. Size estimates are important for the S3 because you might not have enough space left over in your allocation on the S3. **When you make an S3 bucket, please document the data locations on the [DCAN Lab Data Locations & Processing Planning doc](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit) on the Tier 2 tab.**
        36. Tier 1 is for testing. **When you will be doing a large Tier 1 processing job (> 1TB), please put your processing plans on the [DCAN Lab Data Locations & Processing Planning doc](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit) on the Tier 1 tab AND submit a [request form](https://docs.google.com/forms/d/e/1FAIpQLSd1QI_Hmi3khwITVctnaDJYY2M1NegsAWYPR6AXoodUCrrpZw/viewform?usp=sf_link)** about what kinds of data you will be putting onto Tier 1 storage and why Tier 1 is needed, specifically. 
    17. Put the input data where it needs to be. This will more than likely always be on the S3.
        37. If on the S3, make a bucket and give permissions to the appropriate individuals (see section [here](#14-s3)), and sync the data there.
        38. If on tier 1, fill out the request form (if above 1 TB).
14. S3

Read: [s3cmd usage page](https://s3tools.org/usage)


    For a comprehensive introduction to s3, see [this tutorial video](https://drive.google.com/drive/folders/1Oz3i5lbld5VmXGdhWagOMWYOIJmgrJA4) (or [this video](https://drive.google.com/drive/u/1/folders/1Oz3i5lbld5VmXGdhWagOMWYOIJmgrJA4) recorded as part of the Data Processing Workshop hosted for undergraduate research assistants). It is encouraged to watch this tutorial prior to your first time using s3 commands. Note that after you run workbench commands, make sure to module rm workbench prior to doing any s3cmd commands.



    18. Setting up the bucket and basic commands:
        39. To make a new bucket, use: s3cmd mb s3://bucket_name/
        40. To see what is inside the bucket, use: s3cmd ls s3://bucket_name/
        41. To add a file to the bucket, use: s3cmd sync path/to/directory s3://bucket_name/
            19. If trying to upload data to an s3 bucket, make sure to first grab an srun interactive job (2.1.d.)
        42. To remove a file in a bucket, use s3cmd rm s3://bucket_name/ --recursive 
        43. To remove a bucket, use s3cmd rb s3://bucket_name/ --recursive 
        44. To sync a file from your s3 bucket, use: s3cmd sync s3://bucket_name/file_name /path/to/where/you/want/the/directory/to/go/
        45. To add a directory to the bucket, use: s3cmd sync --recursive dir_name s3://bucket_name/ 
            20. If your job runs out of time before the data upload is complete, use s3cmd sync --recursive --no-check-md5 to restart the upload process where it left off and assure everything makes it. Run this even if the sync says complete to validate
        46. To check the size of a single bucket: s3cmd du -H s3://bucket_name/
            21. This process can take awhile and an srun will most likely be needed
        47. To understand how much of your individual s3 quota you have used (each person in the lab is allocated 120TB of s3 space): s3info -u x500
        48. For more s3 command usage, see [here](https://s3tools.org/usage).
    19. Setting an S3 policy on MSI:
        49. Must use this path for your ~/.bashrc to be able to use the set_s3policy command: export PATH=/home/dhp/public/storage/s3policy_bin/:$PATH 

                To open your ~/.bashrc and modify it, use emacs ~/.bashrc &

        50. To check who already has access to the bucket, run: /home/faird/shared/code/internal/utilities/MSI-utilities/s3_get_x500/get_x500.sh s3://bucket_name/ 
            22. Note that this won't include you, so don't forget to include yourself, especially if it isn't your bucket (you can revoke your own access).
        51. Then use this command to set the s3 policy: set_s3policy x500_1,x500_2  bucket_name FULL. The bucket name does not need to have the s3:// prefix in this command 
        52. **IMPORTANT: every time you set a policy you need to list EVERYONE that needs access. You CANNOT just list that one person's x500 or only that one person will have access to the bucket! (Along with the bucket owner)**
    20. Instructions to the manual method of setting an s3 policy on MSI can be found at [this link](https://www.msi.umn.edu/support/faq/how-do-i-use-s3-buckets-share-data-tier-2-storage-other-users). An example policy file that gives full access to the s3 bucket  “bucket_name” is as follows:
15. Optimizing SBATCH parameters 
    21. Run a few subjects with the pipeline. You can use the same 3-5 jobs you used for [storage estimates](#13-storage-allocation-estimates) to estimate job specifications.
    22. Run seff on the jobs 
        53. seff is used to check the memory utilization and CPU efficiency for completed jobs. Note that for running and failed jobs, the efficiency numbers reported by seff are not reliable so please use this tool only for successfully completed jobs.
        54. The job ID can be found first by using  squeue -al –me , and it will be the number associated with the most recent job submission listed under “JOBID”   
            23. Use sacct -S YYYY-MM-DD -u x500 to find job IDs for past jobs that occurred after the specified start date.
        55. Command to run: seff job_id
        56. CPU Efficiency is calculated as the ratio of the actual core time from all cores divided by the number of cores requested divided by the run time.
        57. Memory Efficiency is calculated as the ratio of the maximum amount of RAM used by all tasks divided by the memory requested for the job.
        58. seff output example: 
* _Nodes _can be manipulated in the sbatch by the --nodes flag. 
* _Cores per node_ can be manipulated with the --ntasks-per-node flag. 
* _Job Wall-clock time_ is the length of the job in real hours, and can be manipulated with the -t or --time flags. Based on this output, a time specification of 72:00:00 should suffice. 
* _Memory Utilized _can be manipulated with the --mem flag. Based on this output, a 40 GB specification of memory will allow for a more efficient job versus 60 GB. 
* _CPU Utilized _is the total CPU hours that were allocated based on the request.
* _CPU Efficiency_ is the proportion of the CPU that was utilized for the job compared to the total core-walltime of the CPU.
    23. Recommended sbatch parameters per 1 subject process:

<table>
  <tr>
   <td>
<strong>Pipeline</strong>
   </td>
   <td><strong>SBATCH Parameters and Definition </strong>
   </td>
  </tr>
  <tr>
   <td>fmriprep
   </td>
   <td>Parameters involve grabbing a maximum time of 24 hours on 32 cpus on the msismall partition, with 240gb of total memory. 
<p>
<em>May need increased resources for: multi-echo input data, subjects with more minutes of data than typical ( > 1 hour)</em>
   </td>
  </tr>
  <tr>
   <td>nibabies
   </td>
   <td>Parameters involve grabbing a maximum time of 24 hours on 32 cpus on the msismall partition, with 240gb of total memory. 
<p>
<em>May need increased resources for: multi-echo input data, subjects with more minutes of data than typical ( > 1 hour), or data without precomputed derivatives for segmentation (if JLF segmentation is enabled with “--segmentation-atlases-dir” )</em>
   </td>
  </tr>
  <tr>
   <td>abcd-bids-pipeline
   </td>
   <td>The full job will typically take no more than 48 hours. It will utilize 8 tasks on one node (1 cpu per task) with 20gb of memory and 100gb of temporary storage to run effectively. Recommended partition msismalll.
   </td>
  </tr>
  <tr>
   <td>XCP-D
   </td>
   <td>This job will need 3 hours of maximum wall time, 8 cpus, and 96gb of memory. Recommended partition msismall.
<p>
<em>May need increased resources for: multi-echo input data, subjects with more minutes of data than typical ( > 1 hour)</em>
   </td>
  </tr>
  <tr>
   <td>dcan-infant
   </td>
   <td>The full job will typically take no more than 72 hours and will utilize 24 tasks on one node with 60gb of memory and 100gb of temporary storage. Recommended partition msismall.
   </td>
  </tr>
</table>


*note: all job specifications can increase or decrease based on the amount of data (size or file number) in your dataset. These are recommendations based on job specifications for typical datasets with some buffer added. 



16. Pipelines

<table>
  <tr>
   <td>
<strong>Infants</strong>
   </td>
   <td>Nibabies, DCAN Infant Pipeline, XCPD
   </td>
  </tr>
  <tr>
   <td><strong>Children (4 years and older) and Adults</strong>
   </td>
   <td>Fmriprep, abcd-bids pipeline, XCPD
   </td>
  </tr>
</table>



        Note: the cutoff age for using an adult pipeline depends on the scope of the project


### 
        16.1 Fmriprep {#16-1-fmriprep}


    Read: [fMRIPrep ReadTheDocs site](https://fmriprep.org/en/stable/)


        A NiPreps (NeuroImaging PREProcessing toolS) application for the preprocessing of task-based and resting-state functional MRI (fMRI). For usage information, see [here](https://github.com/DCAN-Labs/fmriprep).



        59. Preferred flags:
            24. --participant-label \ : a space delimited list of participant identifiers or a single identifier (the sub- prefix can be removed)
            25. --cifti-output 91k \ : Possible choices: 91k, 170k. Output preprocessed BOLD as a CIFTI-dense time series. Optionally, the number of grayordinate can be specified (default is 91k, which equates to 2mm resolution). Default: False
            26.  --topup-max-vols 2 \ [**Deprecated as of 22.1.0]**: used to limit frames per run to use from PEPolar fmaps, useful when there are more than one pair of runs that have type _dir-AP _and _dir-PA_ in the /fmap directory to prevent topup distortion correction from taking excessively long. 
            27. --nprocs 32 \ : maximum number of threads across all processes **(if used, this should match the number of cpus allotted for your sbatch job)**
            28. --omp-nthreads 3 \ : maximum number of threads per-process
            29. --resource-monitor \ : enable Nipype’s resource monitoring to keep track of memory and CPU usage
            30. -vv \ : level 2 verbose log output, useful for troubleshooting
            31. -w /work : used to specify a working directory within the same path as the sbatch, named_ /work_.
        60. If an error is encountered, document it [here](https://docs.google.com/document/d/16qSEPV1_FHOHBq2eJOuZLqISv-0zCbpOJQ7HesEQCv4/edit#heading=h.13assk4h50o4). Also, see [Troubleshooting](#23-troubleshooting-and-image-viewing).
        61. Example command:



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")



### 
        16.2 Nibabies {#16-2-nibabies}


    Read: [NiBabies ReadTheDocs site](https://nibabies.readthedocs.io/en/latest/)


        Nibabies is a robust pre-processing MRI and fMRI workflow that is also a part of the NiPreps family. NiBabies is designed and optimized for human infants between 0-2 years old. For more information on Nibabies code, see [here](https://github.com/nipreps/nibabies). For in-depth usage information, one can utilize [this google doc](https://docs.google.com/document/d/1PW8m1tWWqqgKCAJ9XLpJ5tycPB5gqFodrnvNOIavTAA/edit) or see the [Read the Docs here](https://nibabies.readthedocs.io/en/latest/installation.html).



        62. Preferred flags:
            32. --participant-label \ : a space delimited list of participant identifiers or a single identifier (the sub- prefix can be removed)
            33. --age-months \: used to specify the age in months of the participant that is being processed 
            34. --derivatives /derivatives \ : Nibabies will use a segmentation from the segmentation pipeline (pre-postBIBSnet). This flag is used to clarify that the precomputed segmentation directory is being utilized. 
            35. --cifti-output 91k \ : Possible choices: 91k, 170k. Output preprocessed BOLD as a CIFTI-dense time series. Optionally, the number of grayordinate can be specified (default is 91k, which equates to 2mm resolution). Default: False
            36. --topup-max-vols 2 \ [**Deprecated as of 22.2.0] **:used to limit frames per run to use from PEPolar fmaps, useful when there are more than one pair of runs that have type _dir-AP _and _dir-PA_ in the /fmap directory to prevent topup distortion correction from taking excessively long 
            37. -vv \ : level 2 verbose log output, useful for troubleshooting
            38. --nprocs 32 \ : maximum number of threads across all processes
            39. --omp-nthreads 3 \ : maximum number of threads per-process
            40. -w /work : used to specify a working directory within the container’s filesystem, named _/work_.
        63. If an error is encountered, document it [here](https://docs.google.com/document/d/16qSEPV1_FHOHBq2eJOuZLqISv-0zCbpOJQ7HesEQCv4/edit#heading=h.13assk4h50o4). Also, see [Troubleshooting](#23-troubleshooting-and-image-viewing).
        64. Example command:

### 
        16.3 abcd-hcp-pipeline {#16-3-abcd-hcp-pipeline}


    	Read: [abcd-hcp-pipeline @ Github](https://github.com/DCAN-Labs/abcd-hcp-pipeline)


    	Read: [abcd-hcp-pipeline @ ABCC ReadTheDocs](https://collection3165.readthedocs.io/en/stable/pipeline/)


        This pipeline provides an interface for processing BIDS-formatted MRI datasets using the DCAN-HCP pipeline and supporting modules including DCANBOLDProcessing and DCAN Executive Summary. For additional usage, see [here](https://github.com/DCAN-Labs/abcd-hcp-pipeline).

        65. Preferred flags:
            41. --freesurfer-license \ : If using docker or singularity, you will need to acquire and provide your own FreeSurfer license. The license can be acquired by filling out[ this form](https://surfer.nmr.mgh.harvard.edu/registration.html).
            42. --participant-label \ : Optional list of participant IDs to run. Default is all IDs found under the BIDS input directory. The participant label does not include the "sub-" prefix
            43. --stages \ : Specify a subset of stages to run. Can be used to rerun some or all of the pipeline after completing it once, or resume an incomplete runthrough. If a single stage name is given, the pipeline will be started at that stage. If a string with a ":" is given, a stage name before the ":" will tell the pipeline where to start and a stage name after the ":" will tell it where to stop. If no ":" is found, the pipeline will start with the stage specified and run through ExecutiveSummary (or CustomClean/ABCDTask, if specified). Valid stage names: PreFreeSurfer, FreeSurfer, PostFreeSurfer, FMRIVolume, FMRISurface, DCANBOLDProcessing, ExecutiveSummary, CustomClean'
            44. --ncpus \ : Number of cores to use for concurrent processing and algorithmic speedups. Warning: causes ANTs and FreeSurfer to produce non-deterministic results.
            45. --custom-clean \ : Runs DCAN cleaning script after the pipeline completes successfully to delete pipeline outputs based on the file structure specified in the custom-clean JSON. Required for the custom clean stage.
            46. --bandstop \ : Parameters for motion regressor band-stop filter. It is recommended for the boundaries to match the inter-quartile range for participant group respiratory rate (breaths per minute), or to match [BIDS physio data directly](https://www.biorxiv.org/content/10.1101/337360v1). These parameters are highly recommended for data acquired with a frequency of greater than 1 Hz (TR less than 1 second). Default is no filter. Suggested filter ranges can also be found in [this table](https://xcp-d.readthedocs.io/en/latest/generalworkflow.html#id3).
        66. If an error is encountered, document it [here](https://docs.google.com/document/d/1pu6QU32aoKBH1LKNFivYFRvFvZa6lfCETEKYfspy4As/edit). Also, see [Troubleshooting](#23-troubleshooting-and-image-viewing).
        67. Example command: 

### 
        16.4 XCP-D {#16-4-xcp-d}


        The XCP-D workflow takes fMRIPRep, NiBabies, DCAN and HCP outputs in the form of BIDS derivatives. The outputs are required to include at least anatomical and functional outputs with at least one preprocessed BOLD image. For further information, see [here](https://xcp-d.readthedocs.io/en/latest/usage.html). 

1. Preferred flags:
    1. -r &lt;radius> : head radius for computing FD in mm, default 50 is suitable for adult, typically 35-45 for infant
    2. -f 0.3 : framewise displacement threshold for censoring.** Here,** **0.3 is preferred versus a stricter threshold to avoid excluding too much data from the regression model. Stricter thresholding can still be applied when running your analyses on the XCP-D output. **
    3. --cifti : postprocess cifti instead of nifti; this is set default for dcan and hcp input
    4. **[for version 0.2.0 and newer, and “develop” / “unstable” builds dated 10212022 or newer] **--warp-surfaces-native2std : resample surfaces to fsLR space, 32k density, and apply the transform from native T1w to MNI152NLin6Asym space output by fMRIPrep / NiBabies
    5. **[0.3.0 and newer] **--dcan-qc to output QC and executive summary files in similar format to DCAN HCP-based pipelines
    6. -m : add concatenated outputs for functional derivatives
    7. --participant-label : used to specify the specific participant being processed 
    8. --resource-monitor : enable Nipype’s resource monitoring to keep track of memory and CPU usage
    9. --omp-threads 3 : maximum number of threads per-process
    10. --despike : despike the nifti/cifti before postprocessing
    11. -w /work :  used to specify a working directory within the container’s filesystem, named _/work_.
2. If an error is encountered, see [Troubleshooting](#23-troubleshooting-and-image-viewing).
3. Example command: 

### 
        16.5 DCAN Infant Pipeline {#16-5-dcan-infant-pipeline}


    	Read: [infant-abcd-bids-pipeline @ Github](https://github.com/DCAN-Labs/infant-abcd-bids-pipeline)


        This fMRI minimal preprocessing pipeline is based on Washington University's HCP Pipeline. Many changes were made to accomodate the differences in the developing brain of infants. Notably:

    * Skull Stripping: -- This pipeline utilizes ANTs SyN registration. -- This pipeline requires a T2w for skull stripping because the intensity of the CSF is better detected in T2w images.
    * Segmentation: The infant pipeline utilizes ANTs JointFusion. This can be performed using either the T1w image or the T2w image, depending on the quality. (Default is to use T1w.)
    * Surface Reconstruction: These steps in FreeSurfer have been modified:
        * No hires.
        * The aseg is generated from JLF.
        * Adjust class means of tissue to fit T1w contrasts.

        Overview: fMRI -> anatomical registration - no boundary based registration, use T2w to align. Running the PreFreeSurfer, FreeSurfer, and PostFreeSurfer stages will pre-process anatomical images. Following those with fMRIVolume and fMRISurface will pre-process functional images. For more information on the code, see[ the github link here](https://github.com/DCAN-Labs/dcan-infant-pipeline). For a more in-depth description of the pipeline, see [the readthedocs link here](https://dcanlab.readthedocs.io/en/latest/manualpro/infant/pipeline/). 

1. Preferred flags:
    1. --freesurfer-license \ :If using docker or singularity, you will need to acquire and provide your own FreeSurfer license. The license can be acquired by filling out[ this form](https://surfer.nmr.mgh.harvard.edu/registration.html).
    2. --participant-label \ : Optional list of participant IDs to run. Default is all IDs found under the BIDS input directory. The participant label does not include the "sub-" prefix
    3. --session-id \ : filter input dataset by session id. Default is all ids found under the subject input directory(s). A session id does not include "ses-"
    4. --hyper-normalization-method \ :specify the intensity profiles to use for the hyper-normalization step in FreeSurfer: ADULT_GM_IP adjusts the entire base image such that the IP of GM in the target roughly matches the IP of GM of the reference (i.e., the adult freesurfer atlas). Then the WM is shifted in the target image to match the histogram of WM in the reference. ROI_IPS adjusts the intensity profiles of each ROI (GM, WM, CSF) separately and reassembles the parts. NONE skips the hyper-normalization step. This allows the user to run PreFreeSurfer, apply new, experimental hyper-normalization methods and then restart at FreeSurfer. Default: ADULT_GM_IP.
    5. --ncpus \ :  Number of cores to use for concurrent processing and algorithmic speedups. Warning: causes ANTs and FreeSurfer to produce non-deterministic results.
    6. --stage \ : Specify a subset of stages to run. Can be used to rerun some or all of the pipeline after completing it once, or resume an incomplete runthrough. If a single stage name is given, the pipeline will be started at that stage. If a string with a ":" is given, a stage name before the ":" will tell the pipeline where to start and a stage name after the ":" will tell it where to stop. If no ":" is found, the pipeline will start with the stage specified and run through ExecutiveSummary (or CustomClean/ABCDTask, if specified). Valid stage names: PreFreeSurfer, FreeSurfer, PostFreeSurfer, FMRIVolume, FMRISurface, DCANBOLDProcessing, ExecutiveSummary, CustomClean'
1. For further information on DCAN Infant Pipeline flags, see the [readthedocs section here](https://dcanlab.readthedocs.io/en/latest/manualpro/infant/pipeline/#pipeline-optional-flags-and-example).
2. If an error is encountered, document it [here](https://docs.google.com/document/d/1FEcHVf34hWy9o0V0GKka5b2VKw5ndRt8/edit). Also, see [Troubleshooting](#23-troubleshooting-and-image-viewing).
3. Example command:



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")



### 
        16.6 BIBSnet (segmentation pipeline including stages prebibsnet, bibsnet, and postbibsnet) {#16-6-bibsnet-segmentation-pipeline-including-stages-prebibsnet-bibsnet-and-postbibsnet}



1. How to run pipeline outside of CABINET container

            _NOTE: IT IS NOT RECOMMENDED TO RUN BIBSNET OUTSIDE OF THE CONTAINER. THIS IS PURELY FOR TESTING PURPOSES FOR THOSE WORKING ON DEVELOPMENT_


            Run the following script: `/home/feczk001/shared/code/internal/pipelines/CABINET/run_CABINET_no_container.sh`


            When run without arguments, the usage is printed. The script starts by activating the cabinet conda environment

17. S3 Pipeline wrappers

    _Note: see [s3_wrappers_overview.mp4](https://drive.google.com/file/d/1kr9B0ZGZxHGVj4xzl1qMZYY7Qyeg8TYV/view?usp=sharing) for zoom recording of overview presented at DCAN Hackathon_


    Pipeline wrappers exist to submit one subject-session processing job (or analysis) at a time. This allows for improved queueing time of your jobs because you will be requesting less resources per job. Also, partitions have maximum time limits and memory limits that make it almost impossible to run an entire study in one job. The wrapper makes this entire process easier. 


    Below is an overview of the s3 wrapper workflow:


    Set up a wrapper to create the individual jobs per subject and session pair based on the pipeline parameters, the sbatch parameters, and the storage chosen. Gather the correct wrapper script from github: [DCAN-Labs/slurm_pipeline_wrappers](https://github.com/DCAN-Labs/slurm_pipeline_wrappers) or on MSI at /home/faird/shared/code/internal/utilities/slurm_pipeline_wrappers/. Then, copy the wrapper into your own working directory for your project under  /home/{share}/shared/projects/your_project/


    Current list of wrappers developed for s3 integration:

* QSIprep
* NiBabies
* fMRIPrep
* DEAP-derivatives
* CuBids
* DCAN infant wrapper
* abcd-hcp-pipeline (abcd-bids)
* abcd-dcm2bids
* Continuous slurm array submitter 
* s3tos3 file mapper
* Reliability maps
* CABINET

    Each wrapper should have a template file which has generalized commands with variables that get filled with the execution of make_run_files. Modifications will also need to be made within the submit_{}.sh file. 

1. Submit a test job with the wrapper to assure the wrapper is doing what it should do
2. Example: 
    1. ./make_run_files.sh
    2. ./submit_ALL.sh 0-467 (Note: any number scheme may be used provided it is listed as a comma separated list, or as stated in the example, or a combination of both)

**Steps to modify and run pipeline wrappers (using abcd-hcp-pipeline as an example)**



1. Copy the entire folder contents of slurm_abcd-hcp-pipeline_scripts_with_s3_routines_with_ses to your project folder to make changes to these scripts in your own directory
2. Modify sync and copy paths within template.abcd-hcp-pipeline_full_run. This script includes the actual command to run the pipeline using specified data_dir and subject ID.
3. Generate run files by executing make_run_files.sh. This script uses a series of awk commands to pull out subject and session ids from input data within your data bucket and creates a run file for each subject under** **run_files.abcd-hcp-pipeline_full. First review the make_run_files.sh` `script to check that the data directory paths look correct and then execute via ._/make_run_files.sh_. Note that you may need to grab a node first to run this script if you have a large number of subjects.
    1. Before moving on to the submission step, if you are transferring the data via globus, make sure to properly set up the endpoints. See [the section on globus transfers](#25-3-globus) for more information.
4. Submit run files to queue w/ submit_abcd-hcp-pipeline_full_run.sh with the runs as input. For example if you want to submit subjects 0 through 20 and 25, execute the following from the command line: _./submit_abcd-hcp-pipeline_full_run.sh 0-20,25 **Note that you most likely want to test your wrapper on one subject before running multiple.**_
    2. If an error is encountered, see the[ troubleshooting section for s3 wrappers](#23-5-s3-wrapper-troubleshooting-process).
18. File-mapper

    File mapper maps processed outputs into the BIDS format for the abcd-hcp and infant-abcd-bids pipelines. It is designed to help users copy, move, or symlink files from one directory to another using a JSON file as input. The JSON keys are the source of the files and the JSON values are the desired destination path. This program has been used to move important files from a large dataset and would be applicable in any BIDS datasets or other renaming/mapping utility. Another added benefit of file-mapper is that it trims down the amount of files if you delete the original source directory of file-mapper after running it.

1. File-mapper usage:
    1. File-mapper is located in: /home/faird/shared/code/internal/utilities/file-mapper
    2. Select an example mapping json file from the “examples” folder in this location or create your own
    3. Run with the following command: python3 ./file_mapper_script.py &lt;selected json file> -a copy -sp [full output directory of a single subject down to /file] -dp [output dir] -t SUBJECT=[part after “sub-”],SESSION=[part after “ses-”],PIPELINE=[folder name (e.g. abcd-bids)]
        1. PIPELINE refers to the directory your outputs are in which are inside the derivatives folder.
2. See the Example 2 on this repo [here](https://github.com/DCAN-Labs/file-mapper) for a more in-depth explanation.
19. Precision Mapping Wrapper
* Related reading:_ [Rapid Precision Functional Mapping of Individuals Using Multi-Echo fMRI ](https://pubmed.ncbi.nlm.nih.gov/33357444/)_(Lynch et al. 2020)

    **Parcellated functional connectivity (FC)**


        **_Time x FC reliability curve._** The script at  &lt;location> generates a reliability curve from parcellated BOLD timeseries files (.ptseries.nii) in an XCP-D derivatives directory. It is a Slurm sbatch script which requires a list of positional arguments to run. 


        The general form of the run command is 


        sbatch /home/faird/shared/code/internal/analytics/precision_mapping_for_workshop/code_sorted/main_run_script_reliability_curve.sh &lt;DERIVATIVESDIR> &lt;OUTDIR> &lt;SUB> &lt;SES> &lt;TASK> &lt;FD> &lt;NUM> &lt;MIN>   , where: 


            DERIVATIVESDIR = Path to an XCP-D BIDS derivatives directory


            OUTDIR = Directory to write output to  


            SUB = BIDS subject ID (without the “sub-”)


            SES = BIDS session ID (without the “ses-”)


            TASK = BIDS task name (without the “task-”)


            FD = Framewise displacement (FD) threshold, in mm. Is **not **required to be the same FD threshold used with XCP-D. Recommended: between 0.1 and 0.2.


            NUM = Number of permutations to run. (In each permutation, a block of consecutive minutes is sampled   The final reliability values in the curve will be an average over all iterations. Amount of time needed to run increases with the number of iterations.  Recommended: 10 or more.


            MIN = Split-half length limit in minutes, i.e. how far the x-axis of the curve plot extends. Must be an integer. Must be no more than half the total time of TASK specified above.


        _Notes:_

* XCP-D version must be >= 0.3.0
* XCP-D must be run with both --cifti   (CIFTI output) and   --m (concatenate the runs within each BIDS task)
* Currently only Gordon333 parcellated timeseries are supported  (*_space-fsLR_atlas-Gordon_den-91k_timeseries.ptseries.nii).

        **(TBA - FC reliability matrix, dense FC curve, dense FC matrix)**

20. Job Submissions
    24. If an sbatch job is running for >3 minutes, there are most likely no errors in the run command, and job doesn't not need to be closely monitored before completion 
    25. If a submission is above 2000 jobs (slurm’s max in-queue number of jobs), use the continuous submitter.
        68. Path to continuous submitter: /home/faird/shared/code/internal/utilities/slurm_pipeline_wrappers/continuous_slurm_array_submitter
        69. Below is an example of what the submitter script will look like:
    26. Check the [fairshare](#5-fairshare) to know which account to use for processing.
    27. Make sure a ton of jobs aren’t failing right away. Permissions errors, job set up issues, and data issues are common causes.
21. Containers

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


        Note: make sure MIDB’s Virtual Machine is accessible for you before pulling Docker images. [See here to go through the process](#8-virtual-machine-container-building).  

22. Processing Status 

    Produce status html on the output data to see how many processing jobs succeeded and failed (only available for abcd-hcp and infant-abcd-bids pipelines).

1. If your dataset is more than 10 subjects, start an interactive session first: srun -N 1 --cpus-per-task=1  --mem-per-cpu=5gb -A feczk001 -t 6:00:00 -p interactive --x11 --pty bash
2. Load lab-wide miniconda3 environment and activate the abcd-hcp-pipeline_audit: source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

    conda activate abcd-hcp-pipeline_audit

3. Navigate to the correct directory: cd /home/faird/shared/code/internal/utilities/abcd-hcp-pipeline_audit
4. To run on data that's on the s3, s3 keys are needed(run s3info in the command line, then see i.). To run on tier 1, see ii.
    1. S3: python3 run.py --s3_access_key CENSORED --s3_secret_key CENSORED --report_output_dir path/to/the/directory/where/you/want/the/outputs/ s3://bucket_name/ s3://bucket_name/path/to/tier2/outputs group
    2. Tier 1: python3 run.py --report_output_dir path/to/the/directory/where/you/want/the/outputs/ /tier1/path/input/ /tier1/path/output/ group
5. Will produce a s3_status_report.html and .csv. These files give information on the results of each processing step for each subject/session pair. A tip for determining subjects’ processing status counts is to download the .csv into excel, create a pivot table of the data, and provide session ID in the values section and the name of the last column in the rows section. 
23. Troubleshooting and Image Viewing 

    For any processing failures, we triage what happened. Most troubleshooting processes will be needed for infant data, as their processing jobs are more prone to errors. When encountering a processing failure, first check to see if it is already documented in the following pipeline dependent links. It is also necessary and efficient to post your error and run command in #questions or the relevant channel on Slack. If you are unsure what your “relevant channel” is on Slack, ask your supervisor or just default to posting in #questions. 


    Image viewing is a necessary process to conclude if a pipeline job finished successfully, but it also may be necessary when a processing error has been encountered. There are a few different options for viewing anatomical images. It may be most efficient to first download the images locally and then use ITKsnap to view them. However, when working on MSI, applications such as fslview_deprecated or fsleyes can be used. For viewing functional images, wb_view is ideal (make sure to module load fsl or module load workbench before using these applications). For more information on nifti and cifti files (image file types), refer to the video sessions in the subfolders of [this google drive folder](https://drive.google.com/drive/u/0/folders/1yc3w2zNYVZQvTcCgxKk_j6ecZLoyWiCM).



### 23.1 infant-abcd-bids-pipeline (DCAN Infant Pipeline) {#23-1-infant-abcd-bids-pipeline-dcan-infant-pipeline}



1. First follow established SOPs to check that the input data is high quality enough to be processed: see _Structural Pre-Processing Quality Assessment of BIDS _under [Quality Control for Infant Data](https://dcanlab.readthedocs.io/en/latest/manualpro/infant/qc/) on the DCAN Labs RTDs 
2. Check the executive summary to spot errors/quality issues in the pipeline outputs: see _Input Data_ and _Post-Processing Quality Assessment from Executive Summary _under [Quality Control for Infant Data](https://dcanlab.readthedocs.io/en/latest/manualpro/infant/qc/) on the DCAN Labs RTDs 
3. For more in-depth quality assessment that involves inspecting intermediary pipeline outputs, see the [Troubleshooting](https://dcanlab.readthedocs.io/en/latest/manualpro/infant/troubleshooting/) section of DCAN Labs RTDs (includes information on inspecting file outputs from PreFreeSurfer, atlas registration, and functional/structural registration)
4. For issues with dense time series files, see [Checking for NaNs and zeros in dtseries](https://docs.google.com/document/d/1dvpISFRuyKDW0Fc9OCu3GO2GOOlMZ-K8w0PPLtD3oH8/edit) to double check for NaNs

### 
    23.2 abcd-hcp-pipeline (abcd-bids) {#23-2-abcd-hcp-pipeline-abcd-bids}


    In the Infant section above, points c-d can be generalized for ABCD troubleshooting as well, but are typically errors more commonly found with infant vs adult processing. For a generalized spot check of abcd-hcp-pipeline outputs for functional data, use workbench view: 

1. For the abcd-hcp-pipeline functional spot check example, our goal is to get a scaffolding of brain activity. In order to see if brain activity is well represented as functional connectivity, we have to look at the BOLD data (a functional output image). This process can give insights as to whether the processing pipeline has an underlying issue that's been overlooked or if the data has been collected poorly. (Note: workbench works best on MSI’s NICE system; with NICE no ssh into an HPC is required.).
    1. Run module load workbench
    2. Navigate to your output directory. This is an example output directory: cd /home/feczk001/shared/data/ABCC_year2_sites/sub-####/ses-$$$$/files/MNINonLinear/fsaverage_LR32k
    3. Run wb_view then select _Open _and _Load _for all the files
    4. In the _Montage Selection_ section, switch the selected surfaces from _midthickness_ to_ very_inflated_
    5. Deselect all three of the layers in the _Overlay Toolbox/Layers_ section 
    6. Go to _File _and _Open File_, switch the _Files of type_ section to _CIFTI - Dense Data Series Files (*.dtseries.nii)_
    7. Navigate up one directory then down into the _Results _folder
    8. Select a _task_rest dtseries.nii_ and open it 
    9. Go to _File _and _Open File, _and navigate to the parent folder (feczk001/shared/). From here open _ROI_sets_, _Surface_schemes_, _Human_, _Gordon_, and _fsLR_
    10. Switch the _Files of type_ section to _CIFTI - Dense Label Files (*.dlabel.nii)_
    11. Open the _Gordon.networks _file
    12. Back in the _Overlay Toolbox/Layers/File _section, switch the first two files to the _Gordon.networks _file and the _dyconn - task-rest_ file, and check the box under _On_ for those two files
    13. For the _Gordon.networks _file, click the wrench icon under _Settings_ and in the _Labels/Drawing Type _section, select _Outline Label Color_, then select _Close_.

        From here, select points on the brain images to see if the more yellow colors in the _dyconn_ file correspond to the areas outlined by the _Gordon.networks_ file. Focus on distinct networks like the Visual, Motor, and Default Mode. If the seed maps (_dyconn _file) don’t correspond well with the networks, there may be an issue with the data acquisition or the pipeline. For a more in-depth look at this process, see [here](https://umn.app.box.com/file/980329914631). 

2. For data processed with the abcd-hcp-pipeline, the final outputs will always be in the MNINonLinear/fsaverage_LR32k folder, i.e. atlas transformed outputs.
3. Below is an example of what wb_view should look like. The order of the networks seen below are Default Mode, Motor and Visual. The overlays for each all look fairly accurate, as the yellow activation areas correspond to the network region where the voxel was selected. Important to note that one will need to select multiple voxels within a network region in order to identify whether the BOLD activation truly overlays properly with the networks.

### 
    23.3 Nibabies and Fmriprep {#23-3-nibabies-and-fmriprep}


    For a comprehensive document on troubleshooting nibabies and fmriprep errors, [see here](https://docs.google.com/document/u/0/d/16qSEPV1_FHOHBq2eJOuZLqISv-0zCbpOJQ7HesEQCv4/edit).


### 
    23.4 XCP-D {#23-4-xcp-d}


    For common troubleshooting processes and a deeper dive into the utilization of XCP-D, [see here](https://umn.app.box.com/folder/149404140292?s=3gmexhky3rtxafdo118rh1c0c2x35jl6).


### 
    23.5 S3 wrapper troubleshooting process  {#23-5-s3-wrapper-troubleshooting-process}


	**If there are run files:**


    Errors are common when running preliminary tests on s3 wrappers. In order to troubleshoot these errors, the first step is to grab an[ interactive session](#4-1-srun-immediately-run-a-command-using-the-specified-compute-resources). Next, run the content of one run file by copying and pasting the contents into the terminal step-by-step in order to see the outputs of each command in real time, which will uncover the step in which the error is occurring. Once the error is reproduced, incorporate the fix, then rerun make_run_files.sh and a test subject for the submitter script.


    **If there are not run files:**


    The issue should be regarding the paths to the files in make_run_files.sh. With make_run_files.sh opened, copy and paste the contents of the file into the terminal in order to see if the conditional statements match the necessary paths of the files that need to be included. 



24. Fast Track QC Comparison (ABCD)

    The Fast Track QC is used to compare which ABCD subjects contain QC information from one session to the next. The NDA updates this file regularly and we have to compare the differences. The fast track QC comparison can be run on MSI using the compare_2_fastQC_spreadsheets.py script found here: /home/feczk001/shared/code/internal/utilities/abcd-dicom2bids/src/


    Steps for administering the fast track QC comparison process are as follows:

1. Follow the steps on the ReadMe in this [repo](https://github.com/DCAN-Labs/abcd-dicom2bids) to download the latest abcd_fastqc01.csv spreadsheet.
2. Create a folder in /home/rando149/shared/code/internal/utilities/abcd-dicom2bids/spreadsheets/ that’s called fastqc{fill with today's date: YYYYMMDD} 
3. Copy and paste the recently downloaded abcd_fastqc01.txt into the newly created fastqcYYYYMMDD directory.
4. Run source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh
5. Run module load python
6. Run the following command: python3 /home/feczk001/shared/code/internal/utilities/compare_fastQC/compare_2_fastQC_spreadsheets.py /path/to/the/old/fastqc.txt /path/to/the/newly/downloaded/fastqc.txt /path/to/outputs/directory/
    1. The old fast track QC will be the most recently downloaded fast track QC prior to the newly downloaded one. The path will resemble something like this: /home/rando149/shared/code/internal/utilities/abcd-dicom2bids/spreadsheets/old_fastqcYYYYMMDD/abcd_fastqc01.txt 
    2. The path to the newly downloaded fast track QC will resemble something like this:  /home/rando149/shared/code/internal/utilities/abcd-dicom2bids/spreadsheets/new_fastqcYYYYMMDD/abcd_fastqc01.txt 
    3. The output directory will automatically be created in that path specified in the run command, so there is no need to create the directory beforehand. The path should resemble something like this: /home/rando149/shared/projects/ABCC_datalad/name_of_directory
    4. To take a look at the compare_2_fastQC_spreadsheets.py script, see [here](https://github.com/DCAN-Labs/compare-fastQC).
25. Data Uploads and Transfers

    **Before running any processing or analysis that you anticipate will take up more than 1TB of space on tier 1, you must submit [the DCAN Labs Processing and Storage Request Form](https://docs.google.com/forms/d/e/1FAIpQLSd1QI_Hmi3khwITVctnaDJYY2M1NegsAWYPR6AXoodUCrrpZw/viewform). **This form was created to make sure that the DCAN Labs shares stay usable for current employees and also to ensure that others who wish to use our shares do so in a responsible way. Please submit this form BEFORE running processing/analysis jobs. If the associated jobs have already started before the form is filled out, still make sure to fill out the form. 


### 
    25.1 Upload Documentation {#25-1-upload-documentation}

* For data being uploaded to tier 1 storage space, document a description of the dataset and a path to where the data lives, an estimate on the amount of space needed to store the data, the share it’s been uploaded to, the primary owner of the data, and the estimated start and end date for use of the necessary storage space to [this spreadsheet](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=870411543). For further information on allowable data on each share, visit the “Shares” tab on [the above spreadsheet here](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=2135306680).
* For the data being uploaded on the s3, document a description of the data and a path to where the data lives, the project it's associated with, the owner of the s3 bucket, and everyone else who has access to the bucket onto [this spreadsheet](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit). 

    **TO-DO**: in the near future, include what pipeline the dataset was processed with, what version, and the amount of successes/fails and why onto [this spreadsheet](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=0).


### 
    25.2 Data Upload and Transfer Steps {#25-2-data-upload-and-transfer-steps}


    After data has been fully processed, it's important to ask yourself these questions again: where is the data going, and who cares about it?

    28. If it's ABCD data, it is most likely going to be an [NDA upload](https://github.com/DCAN-Labs/nda-bids-upload). 
    29. If the data is being sent to a collaborator, make sure you know who the collaborator is and what method is being used to share the data. Some of DCAN Labs' more frequent collaborators include DEAP, INDI, and UPenn.
    30. If you are trying to upload a small testing dataset (less than 10 subject-session pairs), you will most likely be uploading to Box. Make sure you have already clarified where the data is going on Box.

    Use services like s3 sync, rsync, and globus to sync data / upload data to the appropriate locations. You generally want to use the easiest method for transferring data. Instructions on setting up an .s3cfg are as follows:

1. Create a specific .s3cfg-RANDOM file somewhere in your home directory. You can name it whatever you want, except ".s3cfg". I recommend naming it something easily identifiable for you.
2. Run this command and give the _Access Key ID_ and _Secret Access Key_ when prompted. You should not have to fill in any other fields unless it is hosted by something other than _s3.amazonaws.com_: s3cmd -c ~/.s3cfg-RANDOM --configure                                                Note: the .s3cfg-RANDOM here is the one you made in step one.
3. Test your access through an ls command - the s3cmd -c ~/.s3cfg-RANDOM will have to be part of every command you run to access the buckets that require the credentials you provided.

### 
    25.3 Globus  {#25-3-globus}


	(globus tutorial video isnt uploaded correctly on google drive)


    Using [this link](https://www.globus.org/), follow the instructions to install globus connect personal and log in using UMN credentials. Next, create a personal connect point. Using the globus file manager, choose umnmsi#tier2 on one side and your connect point on the other side.


        **Note**: if you want to upload data from an external hard drive, go to preferences/access and add the hard drive. Visit [this link](https://www.google.com/url?q=https://blogs.iu.edu/ncgas/2021/10/19/adding-an-external-hardrive-to-your-iu-globus-account/&sa=D&source=docs&ust=1667391250182672&usg=AOvVaw3TAVihC6pc10bY2FObWl-4) for a more detailed step-by-step process. 



26. Common Roadblocks on MSI
    31. If you get an error (e.g. about disk space), change your group:
        70. One file: chgrp faird filename
        71. All files in the current directory: chgrp faird *
        72. All files in a specific directory: chgrp faird /home/faird/shared/*