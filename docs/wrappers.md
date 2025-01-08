# S3 Wrappers

_Note: see [s3_wrappers_overview.mp4](https://drive.google.com/file/d/1kr9B0ZGZxHGVj4xzl1qMZYY7Qyeg8TYV/view?usp=sharing) for zoom recording of overview presented at DCAN Hackathon_


Wrappers exist to submit one subject-session job at a time. This allows for improved queueing time of your jobs because you will be requesting less resources per job. Also, partitions have maximum time limits and memory limits that make it almost impossible to run an entire study in one job. The wrapper makes this entire process easier. 


Below is an overview of the s3 wrapper workflow:

![wrapper workflow chart](img/wrapper.png)

A wrapper works by creating individual run files for each subject-session based on a template file. There are 4 necessary files and 2 necessary folders that make up an s3 wrapper:

* `template.<process>`: This file contains all the variables and code needed to pull down the subject data, run the pipeline, and push the outputs to your desired location. 

* `make_run_files.sh`: This script uses a series of `awk` commands to replace the uppercase variable holders with the desired values for each subject. To determine which subjects are to be run, you can either provide a subject list or an s3 bucket and the script will loop through each subject/session pair to create the run files. Here is also where you provide the input and output paths. 

* `resources.<process>`: This script is where the SBATCH parameters are defined.

* `submit.<process>`: This script submits the run files that you specify with an array. 

* `output_logs` (dir): This is where the STDOUT and STDERR files created by SLURM should output to

* `run_files.<process>` (dir): This is where the individual run files are stored. If you need to rerun make_run_files, you do not have to delete these contents first, the script will delete everything in this folder before making new run files. 

Before you create your own wrapper, check our SLURM wrappers repository to see if a wrapper already exists for your process. You can look on GitHub at [DCAN-Labs/SLURM_wrappers](https://github.com/DCAN-Labs/SLURM_wrappers) or on MSI at `/home/faird/shared/code/internal/utilities/SLURM_wrappers/`.

If there is not a pre-existing wrapper for your process, you can use the `wrapper_template` as a starting point. It is recommended to replace the `<process>` placeholder with the name of whatever you are running. **Be sure to check all of the files for this placeholder.** 

**Steps to modify and run a wrapper**

1. Copy the wrapper into your own working directory for your project under `/home/<share>/shared/projects/<your_project>/`

1. Modify the `make_run_files.sh` script to specify your input and output paths. If you are pulling subject/session IDs from a bucket, ensure that the script is looking in the correct location within the bucket. For example, some scripts assume a structure of `s3://<bucket>/niftis/sub-` for the input data. 

2. Ensure that the `template` file has the desired input/output path structure for the sync commands. For example the abcd wrapper assumes the s3 bucket has a structure of `s3://bucket/sorted/sub-`. Also ensure the template is running the most up-to-date command for your pipeline/process. 

4. Execute the make_run_files script to generate the run files. You should be able to see run files being made under the `run_files.<process>` folder. 

    * Note that you may need to grab a compute node first to run this script if you have a large number of subjects.

    * Before moving on to the submission step, if you are transferring the data via globus, make sure to properly set up the endpoints. See [the section on globus transfers](uploads.md#Globus) for more information.

3. Update the resources file for your email, desired group, and computing resources. For peace of mind, it is recommended to provide the full path to the output logs folder.

4. Submit a test job with the wrapper to assure the wrapper is doing what it should do. **Highly recommended to do this before submitting all of your subjects**

5. Submit run files to queue w/ `./submit_<process>.sh` with the runs as input. For example if you want to submit subjects 0 through 20 and 25, execute the following from the command line: `./submit_<process>.sh 0-20,25`.
    
    * Note: any number scheme may be used provided as long as it is listed as a comma separated list, an array, or a combination of both

    * If an error is encountered, see the [troubleshooting section for s3 wrappers](troubleshooting.md#s3-wrappers).


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).