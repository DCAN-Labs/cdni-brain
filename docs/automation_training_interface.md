# Introduction to the Automated Training GUI and How to Use It

The primary objective of creating a GUI was to develop a user-friendly interface to automate and streamline the process of training a deep learning (SynthSeg + nnUNet) algorithm for brain segmentation. The traditional approach prior to the app's creation, which involved manually executing commands in the terminal and extensive copying and pasting of documentation, proved to be very tedious and time-consuming for users. This repetitive process not only introduced numerous potential errors, but also caused significant downtime in the workflow. However, that process still can be utilized by following the documentation in [Segmentation training outside of the GUI](training-nnunet.md), which may be useful to refer to during troubleshooting or training with nnUNet-v2.

The GUI addresses the challenges above by providing an intuitive interface that simplifies and streamlines the training process. This allows users to focus more on the analytical aspects of the process instead of the intricacies of command-line operations.

In this notebook, the different functions of the GUI, created by @Emoney and @Kenevan-Carter for the purpose of automating the training process, will be detailed.

---

## How to Launch the GUI

### Requirements

- **Run this on a persistent desktop within MSI.**
- **Must be within the SynthSeg-fixed-perms environment**:

```bash
source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh
conda activate SynthSeg-fixed-perms
```
To launch the GUI, cd into the Seg-Model-Creation-GUI directory and run:

```bash
python trainer_gui.py
```

Upon start up, you will be prompted to select a preset (more on that below), and choose a pipeline version. As of August 2026, nnUNet-v2 is still under construction and can only be run [outside of the GUI](training-nnunet.md), so nnUNet-v1 should remain selected. 

---

## Functionality

### Parameters

- **Paths**: On launching the main UI window, the user will see a list of path arguments that must be filled out, specific to their own directory locations. Make sure that each directory exists prior to running a training job. Every user will not have access to the same files due to permission errors, so it is important that the user specifies their own paths to the following and has the folder permissions up to date if sharing is needed:
    - **Dcan-nn-unet Path**: Path to your copy of the [dcan-nn-unet github repo](https://github.com/DCAN-Labs/dcan-nn-unet).
    - **SynthSeg Path**: Path to your copy of the CDNI fork of the [SynthSeg github repo](https://github.com/DCAN-Labs/SynthSeg).
    - **Task Path**: Path to the folder containing the train and test data for your specific task. See folder structure below. **This must be placed on `scratch.global`.**
    - **Raw Data Base Path**: Path to the folder that will contain the raw, preprocessed, and cropped data folders for your tasks. This should be two directories above your task folder and named `nnUNet_raw_data_base`. **This must be placed on `scratch.global`.**
    
    Example folder structure:
    ```
    scratch.global/some_general_name/
    ├── nnUNet_raw_data_base
    │   ├── nnUNet_raw_data
    │   │   ├── Task...
    │   │   │   ├── imagesTr
    │   │   │   ├── imagesTs
    │   │   │   ├── labelsTr
    │   │   │   ├── labelsTs
    │   ├── nnUNet_preprocessed
    │   ├── nnUNet_cropped_data
    ```

- **Results Path**: Path to the folder where you want your inferred segmentations and plots to go. CDNI default is `/projects/standard/faird/shared/data/nnUNet-v1/`.
- **Trained Models Path**: Path to the folder where you want to keep the models created during training. CDNI default is `/projects/standard/faird/shared/data/nnUNet-v1/nnUNet_raw_data_base/nnUNet_trained_models/`.

### Arguments

- **Modality**: Specify the modality your dataset is comprised of. Options: `t1`, `t2`, or `t1t2`.
- **Distribution**: Specify the distribution type. Options: `uniform` or `normal`. CDNI default is `uniform`.
- **Task Number**: The task ID that matches up with your task path. CDNI default task naming schema is Task5## for normative infant models, Task6## for anomalous infant models, and Task7## for lifespan models. Please first check the Task numbers in the default folders above before executing a new training job. 
- **Number of SynthSeg Generated Images**: Number of synthetic images SynthSeg should create per age group. An age group is specified by the participants age in months at the beginning of the file name. For each unique age in months in your dataset, SynthSeg will create the number of images specified here, so be mindful of the total unique ages in your dataset. Typical recommendations are <10,000 additional synthetic images. Note also that if training a `t1t2` model then SynthSeg will create an image for both modalities, so the total number of synthetics will double as compared to the `t1` or `t2` modalities. 

### Presets

The user can create custom presets, which streamline the setup process for new training sessions. This feature not only saves time but also reduces the likelihood of errors, ensuring a smoother and more efficient start. Highly recommended to save your configuration as a preset before executing the training job. 

### Check Boxes

Located on the right side of the main UI window are 8 different check boxes. These represent the necessary steps for the model training process. If you want to run the training from start to finish, keep all the boxes selected. Otherwise, you can pick and choose which steps to run. Note that if your job fails during a particular step below in the training process (i.e. due to your MSI connection breaking for any reason), you should clear out the data and files generated from that step and restart from there. 

- **Resize Images**: Formats data uniformly for SynthSeg and nnUNet.
- **Mins/Maxes**: Creates priors for SynthSeg image generation, stored in a `min_maxes` folder within the GUI repo.
- **SynthSeg Image Creation**: Creates synthetic images/labels. These will be placed in a separate folder within your task folder.
- **Copying Over SynthSeg Images**: Moves synthetic data into your corresponding train folders.
- **Create JSON File**: Creates the metadata that nnUNet requires for training, placed in your task folder.
- **Plan and Preprocess**: Preprocess step for nnUNet, extracting necessary info from the dataset.
- **Training the Model**: Trains a model using your training data and saves it to your trained models path.
- **Running Inference**: The trained model will create inferred segmentations for your test data and generate plots in the results folder.

### Tips and Tricks

You can monitor each job in the workflow within a separate terminal. When monitoring the jobs in the workflow, it is helpful to change the account associated with the job ID to the account with the highest Fairshare priority. This is particularly helpful for the resource intense steps like generating synthetics and model training, but can be done for any job in queue. 

- First you will want to check your job to make sure it is still in queue: `squeue -al --me`
- Next, you will use this command to check the Account associated with the job in queue: `sacct`
- Then, you can use this command to check which account has the highest Fairshare value: `sshare -U ${x500}`
- Finally, adjust the job id to the account with the highest Fairshare, so that it can queue faster. As an example: `scontrol update JobId=1234567 Account=feczk001`