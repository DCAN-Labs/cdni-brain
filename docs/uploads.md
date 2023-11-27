# Data Uploads and Transfer

## DCAN Data Transfer Request

If you require assistance or advice from the DCAN computing team on your data transfer, please fill out the [data transfer request form](https://docs.google.com/forms/d/e/1FAIpQLSd84tpEaXS4C9afAneGUGnW6dUtMhS1J9zunWgn5VFQjgRhYA/viewform)

## Data Transfers

1. ABCD data transfers will normally involve an [NDA upload](https://github.com/DCAN-Labs/nda-bids-upload). 

2. We recommend using Box for transferring or sharing small datasets (less than 10 subject-session pairs). Box is a HIPPA compliant secure storage service that is available across many institutions. Box is **not recommended** for large amounts of data. UMN has a [set of guides](https://it.umn.edu/services-technologies/self-help-guides/box-secure-storage-work-files-folders) on how to use Box.

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

Watch [this tutorial](https://drive.google.com/file/d/1Yb_5L9pxIl0fquAtC83XeYleRl_uJo6S/view?usp=drive_link) for how to use globus. More information about setting up globus can be found [here](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-msi-0).

### Tier 1 <-> Tier 2 Transfers
The connect point on the left is the source and the connect point on the right is the target. To transfer data to tier 2 storage (an s3 bucket), using the globus file manager, choose **UMN MSI Tier2** on one side and choose **UMN MSI Home** on the other side. 

### Local <-> Tier 2/Tier 1 Transfers
Using [this link](https://docs.globus.org/globus-connect-personal/), follow the instructions to install globus connect personal and log in using UMN credentials. Next, create a personal connect point. If you want to upload data to/from an external hard drive or Box, go to preferences/access and add the connect point. Visit [this link](https://docs.globus.org/how-to/globus-connect-personal-windows/#configuration) for a more detailed step-by-step process. 

Using the globus file manager, choose **UMN MSI Tier2** or **UMN MSI Home** on one side and your connect point on the other side. Find more information about transferring to tier 2 storage [here](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-second-tier-storage-msi). 
