# Data Uploads and Transfer

## DCAN Data Transfer Request

If you would like to transfer data to/from/between the UMN MSI servers and would like help from the DCAN computing team, see [this page.](requests.md)

## Data Transfers

The data transfer method will depend on how many datasets you are transferring, who you are transferring the data to, and what the data are.

28. Transfers of ABCD data will most likely going include an [NDA upload](https://github.com/DCAN-Labs/nda-bids-upload). 

29. If the data is being sent to a collaborator, make sure you know who the collaborator is and what method is being used to share the data. Some of DCAN Labs' more frequent collaborators include DEAP, INDI, and UPenn.

30. If you are trying to upload a small testing dataset (less than 10 subject-session pairs), you will most likely be uploading to Box. Box is a HIPPA compliant secure storage service that is available across many institutions. Make sure you have already clarified where the data is going on Box. Box is **not recommended** for large amounts of data. UMN has a [set of guides](https://it.umn.edu/services-technologies/self-help-guides/box-secure-storage-work-files-folders) on how to use Box.

Use services like s3 sync, rsync, and globus to sync data / upload data to the appropriate locations. Instructions on setting up an .s3cfg can be found [here.](s3.md)

For information on how to use MSI to transfer/track data, see [this page.](storage.md)

# Tier 1 to Tier 2 Using s3 sync  

s3cmd sync --recursive /path/to/data/folder s3://path/to/s3/target/

Tips: 
1. Leave the trailing slash off of the data folder path

# Using scp to transfer from remote server to Tier 1

This options is best for single folder transfers to Tier 1. From your local computer or the source server (e.g., CMRR's servers), open a terminal. Securely copy the data folder (scp):

scp -o IdentitiesOnly=yes -r /path/to/data/folder umnID@mesabi.msi.umn.edu:/path/to/msi/target/umnID/.

Tips: 
1. You will need to duo to initiate the connection and start the transfer. 
2. Leave the trailing slash off of the data folder path.
3. Transfers are often more successful when transferring to your MSI home directory than a shared directory (e.g., home/faird/shared). Once the data are in your home directory you can move the data to a shared directory with a mv command. 

# Globus

Watch [this tutorial](https://drive.google.com/file/d/1Yb_5L9pxIl0fquAtC83XeYleRl_uJo6S/view?usp=drive_link) for how to use globus.


Using [this link](https://docs.globus.org/globus-connect-personal/), follow the instructions to install globus connect personal and log in using UMN credentials. Next, create a personal connect point. To transfer data to tier 2 storage (an s3 bucket), using the globus file manager, choose **UMN MSI Tier2** on one side and your connect point on the other side. Find more information about transferring to tier 2 storage [here](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-second-tier-storage-msi). If you are wanting to transfer data to tier 1 storage, choose **UMN MSI Home** as your location. More information about setting up globus can be found [here](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-msi-0).


**Note**: if you want to upload data from an external hard drive, go to preferences/access and add the hard drive. Visit [this link](https://docs.globus.org/how-to/globus-connect-personal-windows/#configuration) for a more detailed step-by-step process. 
