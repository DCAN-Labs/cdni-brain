# Manual Training nnUNet

This page provides the code on how to train segmentation models manually outside of the streamlined GUI. You will need to grab [a srun](slurm-params.md#srun) to run these commands. You will also need to [load the labwide miniconda environment](miniconda.md) and activate the `SynthSeg-fixed-perms` conda environment. You will also need to have stable local copies of these two github repositories: [dcan-nn-unet](https://github.com/DCAN-Labs/dcan-nn-unet) and [SynthSeg](https://github.com/DCAN-Labs/SynthSeg). Currently, there are copies of all necessary code available here: `/projects/standard/faird/shared/code/stable/utilities/segmentation_model_training/`. These copies will be used for the examples below (set as the variable `$stable_code_path=/projects/standard/faird/shared/code/stable/utilities/segmentation_model_training/`), but they are stable versions and should **not** be altered. Any testing should be done on your own local copies of the code. Run the set of commands below after setting up your dataset on `scratch.global` with the following example data structure:

```
    scratch.global/some_general_name/
    ├── nnUNet_raw_data_base
    │   ├── nnUNet_raw_data
    │   │   ├── Task###
    │   │   │   ├── imagesTr
    │   │   │   ├── imagesTs
    │   │   │   ├── labelsTr
    │   │   │   ├── labelsTs
    │   ├── nnUNet_preprocessed
    │   ├── nnUNet_cropped_data
```

**Resize images**

```
source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

conda activate SynthSeg-fixed-perms

cd ${stable_code_path}
```

Rename current subfolders under Task### to "Old_$$$$$" (ex: Old_imagesTr, Old_imagesTs, etc.), and make new empty copies with the original name to be used as destination folders.

```
python ${stable_code_path}/dcan-nn-unet/dcan/img_processing/resize_images.py /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/Old_labelsTr/ /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/labelsTr/	
```

**Create min/maxes (hard coded paths):**

```
ssh -Y agate

srun --time=96:00:00 --mem=128GB --tmp=40gb -p msismall -A $account --x11 --pty bash

source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

conda activate SynthSeg-fixed-perms

cd ${stable_code_path}/SynthSeg

export PYTHONPATH=${PYTHONPATH}:${stable_code_path}/SynthSeg/

export PYTHONPATH=${PYTHONPATH}:${stable_code_path}/SynthSeg/SynthSeg/

python ./SynthSeg/dcan/ten_fold_uniformity_estimation_test.py
```

**Running SynthSeg:**

```
ssh -Y agate

srun --time=96:00:00 --mem=256GB --tmp=80gb -p msismall -A $account --x11 --pty bash

cd ${stable_code_path}/SynthSeg

export PYTHONPATH=${PYTHONPATH}:${stable_code_path}/SynthSeg/

export PYTHONPATH=${PYTHONPATH}:${stable_code_path}/SynthSeg/SynthSeg/

python ./SynthSeg/dcan/image_generation_for_all_ages.py /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/ /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/SynthSeg_generated/ ./SynthSeg/data/labels_classes_priors/dcan/uniform/###/mins_maxes.npy 1000 --distribution="uniform"
```

**Setting up running one fold of nnUNet:**

First - copy over Synthseg images to training folders:

```
source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

conda activate SynthSeg-fixed-perms

cd ${stable_code_path}/dcan-nn-unet

python ./dcan/util/copy_over_augmented_image_files.py /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/SynthSeg_generated/images/ /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/imagesTr/ /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/labelsTr/

python ./dcan/util/copy_over_augmented_image_files.py /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/SynthSeg_generated/labels/ /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/imagesTr/ /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/labelsTr/

rm -r /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/SynthSeg_generated/
```

***Create dataset.json for nnUNet plan and preprocess:***

```
cd /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/

export PYTHONPATH=${PYTHONPATH}:${stable_code_path}/dcan-nn-unet/

export nnUNet_raw_data_base="/scratch.global/some_general_name/nnUNet_raw_data_base/"

export nnUNet_preprocessed="/scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_preprocessed/"

export RESULTS_FOLDER="/projects/standard/faird/shared/data/nnUNet-v1/nnUNet_raw_data_base/nnUNet_trained_models/"

python ${stable_code_path}/dcan-nn-unet/dcan/dataset_conversion/create_json_file.py Task###

export PYTHONPATH=${PYTHONPATH}:${stable_code_path}/dcan-nn-unet/dcan/

python ${stable_code_path}/dcan-nn-unet/dcan/dataset_conversion/fix_json_file.py ./dataset.json ./dataset2.json
```

Finally, remove comma after vermis in `dataset2.json`, then delete `dataset.json` and save `dataset2.json` as `dataset.json`

***Run plan and preprocess:***

```
cd /projects/standard/faird/shared/code/internal/nnUNet-v1/slurm_scripts_stable/

sbatch NnUnet_plan_and_preprocess_agate.sh /scratch.global/some_general_name/nnUNet_raw_data_base/ ### /projects/standard/faird/shared/data/nnUNet-v1/nnUNet_raw_data_base/nnUNet_trained_models/
```

***Run nnUNet train:***

```
cd /projects/standard/faird/shared/code/internal/nnUNet-v1/slurm_scripts_stable/

sbatch NnUnetTrain_agate.sh ${fold_number} ${Account} ### /scratch.global/some_general_name/nnUNet_raw_data_base/ /projects/standard/faird/shared/data/nnUNet-v1/nnUNet_raw_data_base/nnUNet_trained_models/ [-c]
```

Only include [-c] if continuing a job. Monitor the logs - once fold 0 hits epoch 1 then you can submit the remaining folds (i.e. folds 1-4)

**Run nnUNet predict:**

This step is to be done after training folds have completely finished (you should see a `model_final_checkpoint.model` zip file and pkl file in each fold directory in the results folder).

```
cd /projects/standard/faird/shared/code/internal/nnUNet-v1/slurm_scripts_stable/

sbatch infer_agate.sh
```

`infer_agate.sh` is hard coded, so edit your paths accordingly. 

**Optional - Review the inferred segmentations (we usually have too many):**

```
fslmerge -t ${inferred_segs_dir} `ls *.nii.gz`
```

**Calculate dice scores and create plots**

Remove the inferred_segs_all file if necessary and run the following commands:

```
ssh -Y agate

srun --time=24:00:00 --mem=32GB --tmp=20gb -p interactive -A ${Account} --x11 --pty bash

cd ${stable_code_path}/SynthSeg/SynthSeg/dcan/paper

source /projects/standard/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

conda activate SynthSeg-fixed-perms

export PYTHONPATH=${PYTHONPATH}:${stable_code_path}/SynthSeg

python evaluate_results.py /scratch.global/some_general_name/nnUNet_raw_data_base/nnUNet_raw_data/Task###/labelsTs/ /scratch.global/some_general_name/predict_test/ /scratch.global/some_general_name/predict_results/
```

For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).