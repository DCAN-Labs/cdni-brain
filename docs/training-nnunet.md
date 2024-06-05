# Training nnUNet

This page provides the code on how to train nnunet. You will need to grab [a srun](slurm-params.md#srun) to run these commands. You will also need to [load the labwide miniconda environment](miniconda.md) and activate the SynthSeg conda environment. 

Create min/maxes (hard coded paths):

```
source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh
conda activate SynthSeg
cd ~/SynthSeg

export PYTHONPATH=${PYTHONPATH}:/home/faird/lundq163/SynthSeg/
export PYTHONPATH=${PYTHONPATH}:/home/faird/lundq163/SynthSeg/SynthSeg/

python ./SynthSeg/dcan/ten_fold_uniformity_estimation_test.py
```

Running SynthSeg:

```
cd ~/SynthSeg
export PYTHONPATH=${PYTHONPATH}:/home/faird/lundq163/SynthSeg/
export PYTHONPATH=${PYTHONPATH}:/home/faird/lundq163/SynthSeg/SynthSeg/

python ./SynthSeg/dcan/image_generation_for_all_ages.py /scratch.global/lundq163/nnUNet_HBCD/nnUNet_raw_data_base/nnUNet_raw_data/Task528/ /scratch.global/lundq163/nnUNet_HBCD/nnUNet_raw_data_base/nnUNet_raw_data/Task528/SynthSeg_generated/ /home/faird/lundq163/SynthSeg/data/labels_classes_priors/dcan/uniform/528/mins_maxes.npy 2000 --distribution="uniform"

python ./SynthSeg/dcan/image_generation_for_all_ages.py /scratch.global/lundq163/nnUNet_HBCD/nnUNet_raw_data_base/nnUNet_raw_data/Task527_HBCD/ /scratch.global/lundq163/nnUNet_HBCD/nnUNet_raw_data_base/nnUNet_raw_data/Task527_HBCD/SynthSeg_generated/ /home/faird/lundq163/SynthSeg/data/labels_classes_priors/dcan/uniform/527/mins_maxes.npy 1000 --distribution="uniform"

python ./SynthSeg/dcan/image_generation_for_all_ages.py /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task700_T1_T2_Fold0/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task700_T1_T2_Fold0/SynthSeg_generated/ /home/faird/lundq163/SynthSeg/data/labels_classes_priors/dcan/uniform/mins_maxes_fold_0.npy 1000 --distribution="uniform"
```

Running one fold of nnUNet:
First - copy over synthseg images to training folders:

```
python copy_over_augmented_image_files.py /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/SynthSeg_generated/images/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/imagesTr/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/labelsTr/

python copy_over_augmented_image_files.py /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/SynthSeg_generated/labels/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/imagesTr/ /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/labelsTr/

cd /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task70X/
mv ./imagesTr/*_SynthSeg_generated_0000.nii.gz ./labelsTr/ -v
mv ./imagesTr/*_SynthSeg_generated_0001.nii.gz ./labelsTr/ -v
ls ./imagesTr/ | wc -l
ls ./labelsTr/ | wc -l
rm SynthSeg_generated/ -r
```
imagesTr should have 18162, and labelsTr should have 9081, and then SynthSeg_generated can be removed


Create dataset json for nnunet plan and preprocess:

```
cd /scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_raw_data/Task700/

export PYTHONPATH=${PYTHONPATH}:/home/faird/lundq163/dcan-nn-unet/
export nnUNet_raw_data_base="/scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/"
export nnUNet_preprocessed="/scratch.global/lundq163/nnUNet/BOBSnet_raw_data_base/nnUNet_preprocessed/"
export RESULTS_FOLDER="/home/feczk001/shared/data/nnUNet/nnUNet_raw_data_base/nnUNet_trained_models"

python /home/faird/lundq163/dcan-nn-unet/dcan/dataset_conversion/create_json_file.py TaskXXX

export PYTHONPATH=${PYTHONPATH}:/home/faird/lundq163/dcan-nn-unet/dcan/

python /home/faird/lundq163/dcan-nn-unet/dcan/dataset_conversion/fix_json_file.py ./dataset.json ./dataset2.json
'next remove comma after vermis in dataset2, then delete dataset and save dataset2 as dataset'
```

Run plan and preprocess:

```
cd /home/faird/shared/code/internal/nnUNet/slurm_scripts/70X
sbatch NnUnet_plan_and_preprocess_agate.sh
```