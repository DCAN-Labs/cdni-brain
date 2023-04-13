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

    - `-B /path/to/code/file.py:/path/in/cabinet`

    - To find path in cabinet use `singularity shell --nv container_path.sif` (note flags on shell command need to match the .sh file singularity run flags)

### Edit sif file:

1. Needs to be the most up to date version

1. [Instructions on how to pull the container](https://3.basecamp.com/5032058/buckets/21517584/messages/4239484815)

1. `singularity pull cabinet.sif docker://dcanumn/cabinet:version-tag`

### Edit App flags:

1. `--participant-label` (number after sub- for the participant you want to run)

1. Start and end flags

    - `--start prebibsnet` (usually start here)

    - `--end postbibsnet` (change depending on how much you want to run)

1. `--work` (optional, depends on what you are testing)

## Prepare testing directories

These are the directories you specified in the `cabinet_testing.sh` file

    `cd /home/feczk001/shared/projects/segpipeline_testing/`

    `mkdir my_test_dir`

    `cd my_test_dir`

    `mkdir input` (optional, can use segpipeline/input; either way make sure appropriate inputs are in here)

    `mkdir derivatives`

    `mkdir logs`

    `mkdir work` (optional, if you don’t need to see this for your testing, you don’t need this)

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

Check out the [CABINET documentation](https://github.com/DCAN-Labs/CABINET/blob/main/README.md) and double check that you didn't miss anything in your command

Ensure that the input data is [BIDS](dcm2bids.md) valid
  * [Run CuBIDS on the dataset](bids.md)

  * If it isn't BIDS valid, fix the BIDS validity errors and run CABINET again

Look at anatomical input BIDS filenames to see if there are a mix of rec-normalized and regular T1w/T2ws:
  * This can lead to the average T1w and/or T2w that is not meaningful and will not work well within CABINET.

  * We want either all rec-normalized T1ws and/or T2ws or all regular T1w/T2ws, not both.

  * If the anat directory includes both rec-normalized and regular T1w/T2ws, remove one of the options and re-run CABINET (this will be taken care of with a .bidsignore eventually, but it is currently a manual process).

Look at anatomical images to check the quality of the input anatomical images
  * This will mostly be motion artifacts.

  * If there are poor quality images, remove those images and re-run CABINET.

Look at the averaged T1w and T2w to ensure it looks “normal” (`derivatives/prebibsnet/sub-*/ses-*/averaged/sub-*_ses-*_000(0|1).nii.gz`)
  * There shouldn’t be “bad” average anatomical images here if all of the above conditions are satisfied.

  * If there are, post a Github issue [here](https://github.com/DCAN-Labs/CABINET/issues).

Look at the cropped T1w and T2w to ensure it is not overcropped or undercropped. (`derivatives/prebibsnet/sub-*/ses-*/cropped/T(1|2)w/sub-*_ses-*_000(0|1).nii.gz`)
  * If it is overcropped or undercropped, [add a brain_z_size column to your participants.tsv](https://github.com/DCAN-Labs/CABINET#participantstsv) with z sizes more appropriate for your dataset/subjects/study and re-run CABINET

Look at both resizing outputs to see if either alignment (xfms[non-ACPC] or ACPC_align) option leads to a properly aligned T1w and T2w (`derivatives/prebibsnet/sub-*/ses-*/resized/(ACPC_align|xfms)/`)
  * If neither image pair looks aligned to each other, post a Github issue [here](https://github.com/DCAN-Labs/CABINET/issues). We only need one of the pairs to be aligned to each other. It’s okay if one of them looks bad, we’re making a choice in the next step.

  * Note: be sure to check different areas throughout the brain.

Look at the BIBSnet input and ensure it chose the properly aligned pair of T1w and T2w images. (`derivatives/bibsnet/sub-*/ses-*/input/`)
  * To do this, I open the corresponding files in the `derivatives/prebibsnet/sub-*/ses-*/resized/(ACPC_align|xfms)/` and `derivatives/bibsnet/sub-*/ses-*/input/` to ensure the T1ws match the T1ws and the T2ws match the T2ws.

  * If it chose the wrong pair, post a Github issue [here](https://github.com/DCAN-Labs/CABINET/issues).

  * For the sake of time, you can copy and rename the properly aligned image pair into that directory, mirroring the naming schema present in that directory.

Look at the BIBSnet output and ensure there are not any holes in the segmentation. (`derivatives/bibsnet/sub-*/ses-*/output/`)
  * Load into your image viewer of choice the BIBSnet input file and the BIBSnet output file with the segmentation on top. 

  * If there are holes in the segmentation and you used the correct resources for you job, post a Github issue [here](https://github.com/DCAN-Labs/CABINET/issues).

  * It is okay if the chirality does not match, this will *hopefully* be fixed in postBIBSnet.

Look at the chirality corrected outputs to ensure the chirality matches the proper hemisphere and there are no holes. (`/derivatives/precomputed/sub-*/ses-*/chirality_correction/`)
  * If the segmentation doesn’t have the proper chirality, post a Github issue [here](https://github.com/DCAN-Labs/CABINET/issues).
  
  * If the segmentation has holes, post a Github issue [here](https://github.com/DCAN-Labs/CABINET/issues).

Look at the precomputed file to make sure it’s aligned to the T1 (T1 and T2 model, T1-only model) or the T2 (T2-only model).
  * If misaligned, post a Github issue [here](https://github.com/DCAN-Labs/CABINET/issues).
