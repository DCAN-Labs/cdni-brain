# Data Uploads and Transfer

## DCAN Data Transfer Request

If you require assistance or advice from the DCAN computing team on your data transfer, please fill out the [data transfer request form](https://docs.google.com/forms/d/e/1FAIpQLSd84tpEaXS4C9afAneGUGnW6dUtMhS1J9zunWgn5VFQjgRhYA/viewform)

## Data Transfers

1. ABCD data transfers will normally involve an [NDA upload](https://github.com/DCAN-Labs/nda-bids-upload). 

2. We recommend using Box for transferring or sharing small datasets (less than 10 subject-session pairs). Box is a HIPPA compliant secure storage service that is available across many institutions. Box is **not recommended** for large amounts of data. UMN has a [set of guides](https://it.umn.edu/services-technologies/self-help-guides/box-secure-storage-work-files-folders) on how to use Box.

Use services like s3 sync, rsync, and globus to sync data / upload data to the appropriate locations. You generally want to use the easiest method for transferring data. Instructions on setting up an .s3cfg can be found [here.](s3.md)

For information on how to use MSI to transfer/track data, see [this page.](storage.md)

## Globus

Watch [this tutorial](https://drive.google.com/file/d/1Yb_5L9pxIl0fquAtC83XeYleRl_uJo6S/view?usp=drive_link) for how to use globus. More information about setting up globus can be found [here](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-msi-0).

### Tier 1 <-> Tier 2 Transfers
The connect point on the left is the source and the connect point on the right is the target. To transfer data to tier 2 storage (an s3 bucket), using the globus file manager, choose **UMN MSI Tier2** on one side and choose **UMN MSI Home** on the other side. 

### Local <-> Tier 2/Tier 1 Transfers
Using [this link](https://docs.globus.org/globus-connect-personal/), follow the instructions to install globus connect personal and log in using UMN credentials. Next, create a personal connect point. If you want to upload data to/from an external hard drive or Box, go to preferences/access and add the connect point. Visit [this link](https://docs.globus.org/how-to/globus-connect-personal-windows/#configuration) for a more detailed step-by-step process. 

Using the globus file manager, choose **UMN MSI Tier2** or **UMN MSI Home** on one side and your connect point on the other side. Find more information about transferring to tier 2 storage [here](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-second-tier-storage-msi). 
