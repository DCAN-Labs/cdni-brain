# Fast Track QC Comparison (ABCD)

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