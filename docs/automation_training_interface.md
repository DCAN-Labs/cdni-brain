# Introduction to the Automated Training GUI and How to Use It

The primary objective of creating a GUI was to develop a user-friendly interface to automate and streamline the process of training a deep learning algorithm for brain segmentation. The traditional approach prior to the app's creation, which involved manually executing commands in the terminal and extensive copying and pasting of documentation, proved to be very tedious and time-consuming for users. This repetitive process not only introduced numerous potential errors, but also caused significant downtime in the workflow.

The GUI addresses these challenges by providing an intuitive interface that simplifies and accelerates the training process. This allows users to focus more on the analytical aspects of the process instead of the intricacies of command-line operations.

In this notebook, the different functions of the GUI, created by @Emoney and @Kenevan-Carter for the purpose of automating the training process, will be detailed.

---

## Functionality

### Parameters

- **Paths**: On launching the main UI window, the user will see a list of path arguments that must be filled out, specific to their own directory locations. Every user will not have access to the same files due to permission errors, so it is important that the user specifies their own paths to the following:
    - **Dcan-nn-unet Path**: Path to your dcan-nn-unet repo.
    - **SynthSeg Path**: Path to your SynthSeg repo.
    - **Task Path**: Path to the folder containing the train and test data for your specific task.
    - **Raw Data Base Path**: Path to the folder that will contain the raw, preprocessed, and cropped data folders for your tasks. This should be a couple directories above your task folder.
    
    Example folder structure:
    ```
    some_general_name/
    ├── nnUNet_raw_data_base
    │   ├── nnUNet_raw_data
    │   │   ├── Task...
    │   │   │   ├── imagesTr
    │   │   │   ├── imagesTs
    │   │   │   ├── labelsTr
    │   │   │   ├── labelsTs
    │   │   ├── Task...
    │   ├── nnUNet_preprocessed
    ├── nnUNet_cropped_data
    ```

- **Results Path**: Path to the folder where you want your inferred segmentations and plots to go.
- **Trained Models Path**: Path to the folder where you want to keep the models created during training.

### Arguments

- **Modality**: Specify the modality your dataset is comprised of. Options: `t1`, `t2`, or `t1t2`.
- **Distribution**: Specify the distribution type. Options: `uniform` or `normal`.
- **Task Number**: The task ID that matches up with your task path.
- **Number of SynthSeg Generated Images**: Number of synthetic images SynthSeg should create per age group.

### Presets

The user can create custom presets, which streamline the setup process for new training sessions. This feature not only saves time but also reduces the likelihood of errors, ensuring a smoother and more efficient start.

### Check Boxes

Located on the right side of the main UI window are 8 different check boxes. These represent the necessary steps for the model training process. If you want to run the training from start to finish, keep all the boxes selected. Otherwise, you can pick and choose which steps to run.

- **Resize Images**: Formats data uniformly for SynthSeg and nnUNet.
- **Mins/Maxes**: Creates priors for SynthSeg image generation, stored in a `min_maxes` folder within the GUI repo.
- **SynthSeg Image Creation**: Creates synthetic images/labels. These will be placed in a separate folder within your task folder.
- **Copying Over SynthSeg Images**: Moves synthetic data into your corresponding train folders.
- **Create JSON File**: Creates the metadata that nnUNet requires for training, placed in your task folder.
- **Plan and Preprocess**: Preprocess step for nnUNet, extracting necessary info from the dataset.
- **Training the Model**: Trains a model using your train data and saves it to your trained models path.
- **Running Inference**: The trained model will create inferred segmentations for your test data and generate plots in the results folder.

---

## How to Launch the GUI

### Requirements

- **Run this on a persistent desktop within MSI.**
- **Must be within the SynthSeg-fixed-perms environment**:

```bash
source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh
conda activate SynthSeg-fixed-perms
```
To launch the GUI, cd into the Seg-Model-Creation-GUI directory and run:


```bash
python automation_gui.py
```
Alternatively, you can run:
```bash
python /path/to/automation_gui.py
```
