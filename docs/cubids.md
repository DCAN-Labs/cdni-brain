# CuBIDS (Curation of BIDS)

Used to make sure the data is BIDS valid (utilizing the [bids-validator package 1.7.2](https://cubids.readthedocs.io/en/latest/installation.html#:~:text=Now%20that%20we,%24), which will already be installed on MSI) and to ensure all of the acquisition parameters of the data are what you expect them to be. If you are running CuBIDS on data that has more than ten subjects, then use an [srun ](#4-1-srun-immediately-run-a-command-using-the-specified-compute-resources)or an [sbatch](#4-2-sbatch-copies-the-script-in-an-internal-storage-and-then-uploads-it-on-the-compute-node-when-the-job-starts).

14. Load CuBIDS environment: 

    - Load lab-wide miniconda3 environment: 

        source /home/faird/shared/code/external/envs/miniconda3/load_miniconda3.sh

        - This ensures that you have all of the proper packages installed to run cuBIDS.

    - Activate cuBIDS environment

        conda activate cubids

    - If this method did not work, see CuBIDS documentation [here](https://cubids.readthedocs.io/en/latest/installation.html).

1. Run CuBIDS on your dataset:

    - Validation: cubids-validate --sequential /bids/directory/path/ /output/path/with-prefix-at-the-end

        - To determine if your file-naming and directory structure is proper BIDS

        - To determine if any BIDS required files or metadata fields are missing.

        - The sequential flag treats all subjects as its own BIDS dataset. You won’t see errors about subjects having differing sessions or parameters, which can be good or bad depending on what the data is.

        - Note: the /bids/directory/path/ is the full path to the BIDS data set directory containing your subjects. The /output/path/with-prefix-at-the-end is the full path to the CuBIDS validate output file, which will have a prefix at the end. The recommended prefix to use is the date and the study as CuBIDS validate may need to be run many times.

    - Group: cubids-group /bids/directory/path/ /output/path/with-prefix-at-the-end

        - To group files according to acquisition, and therefore, confirming the data was acquired how you expected.

        - To make sure all fieldmaps have IntendedFors and that all bold runs have fieldmaps paired with them for distortion correction.

        - Note: the /bids/directory/path/ is the full path to the BIDS data set directory containing your subjects. The /output/path/with-prefix-at-the-end is the full path to the CuBIDS group output file, which will have a prefix at the end. The recommended prefix to use is the date and the study as CuBIDS group may need to be run many times.

    - add-nifti-info can be used in the same manner as group to add information contained in the niFti file headers to the sidecar JSONs. add-nifti-info won't duplicate nifti header information in sidecar files if that information already exists.

    - Note: /output/path/with-prefix-at-the-end can be any path that already exists that you want CuBIDS to output it’s findings to. You need to make sure this directory exists prior to running CuBIDS. You can also specify any prefix you want to identify that specific run of cubids-validate or cubids-group. Ex: 2022-10-04 (CuBIDS automatically adds a separating character.

    - For more information on CuBIDs and running it, see the documentation [here](https://cubids.readthedocs.io/en/latest/index.html).