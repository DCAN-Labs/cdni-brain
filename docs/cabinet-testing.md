# Testing CABINET
## Make changes to the code
- Write code however you want then put the changes (use github!) on `/home/feczk001/shared/code/internal/pipelines/CABINET`
    - check that nobody else is using this directory for other testing. If they are you can use the miran045 share

- Dont use `/home/faird/shared/code/internal/pipelines/CABINET` because its the stable version

## Change the test container
`cd /home/faird/shared/code/internal/pipelines/cabinet_container/`

Duplicate `cabinet_testing.sh`

### Edit sbatch:

1. `SBATCH –mail-user=YOUR EMAIL`

1. `SBATCH -o /path/to/logs/dir` (has to exist already)

1. `SBATCH -e /path/to/logs/dir` (can be same as above)

1. Might need to edit `SBATCH -A` for fairshare optimization

1. Might have to edit `SBATCH -p` for other partitions when using Mesabi/Agate/Mangi

### Edit Binds:

1. Input bind: `-B /path/to/input/:/input \ `

1. Output bind: `-B /path/to/output/:/output \`

1. Might need to edit the code binds depending on what code changes you made and where:

    1. `-B /path/to/code/file.py:/path/in/cabinet`

    1. To find path in cabinet use `singularity shell --nv container_path.sif` (note flags on shell command need to match the .sh file singularity run flags)

### Edit sif file:

1. Needs to be the most up to date version

1. [Instructions on how to pull the container](https://3.basecamp.com/5032058/buckets/21517584/messages/4239484815)

1. `singularity pull cabinet.sif docker://dcanumn/cabinet:version-tag`

### Edit App flags:

1. `--participant-label` (number after sub- for the participant you want to run)

1. Start and end flags

    1. `--start prebibsnet` (usually start here)

    1. `--end postbibsnet` (change depending on how much you want to run)

1. `--work` (optional, depends on what you are testing)

## Prepare testing directories

These are the directories you specified in the `cabinet_testing.sh` file

`cd /home/feczk001/shared/projects/segpipeline_testing/`

`mkdir my_test_dir`

`cd my_test_dir`

`mkdir input` 

- Optional, can use `segpipeline_testing/input`

- Either way, make sure the appropriate inputs are in here.

`mkdir derivatives`

`mkdir logs`

`mkdir work`
- (optional, if you don’t need to see this for your testing, you don’t need this)

Run the `test.sh`

Log into Mesabi/Mangi/Agate

`sbatch cabinet_testing.sh`

You will get emails about how it's going. 
- Make sure you put your email in the steps above!

Your logs will go to the logs path specified above
- make sure this directory exists!

## Did it work?

Once it's done running, you’ll get an email that says **COMPLETED** or **FAILED**

### **COMPLETED**: what else do you need to check?

Go to the derivatives folder you specified above
- `my_test_dir/derivatives/bibsnet/sub-380056/ses-4mo/anat`

Make sure the derivatives exist! They'll look something like this:

- `sub-123456_ses-session_space-orig_desc-aseg_dseg.nii.gz`

- `sub-123456_ses-session_space-orig_desc-brain_mask.nii.gz`

Spot check them

1. `module load fsl`

1. `fsleyes sub-123456_ses-session_space-orig_desc-aseg_dseg.nii.gz sub-123456_ses-session_space-orig_desc-brain_mask.nii.gz &`

    Do this in an openondemand session if you weren’t already using it

1. Load in the T1 “add from file”

1. Change color of aseg (MGH Cortical or MGH subcortical)

1. Put the t1 on bottom and the aseg on top.

1. See if there are any glaring holes/flaws

### **FAILED**: how do I fix it?

Look inside the `.err` and `.out` files in the specified log directory
