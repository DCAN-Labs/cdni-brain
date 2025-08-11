# Data Uploads and Transfer

**DCAN Data Transfer Request**

If you require assistance or advice from the DCAN computing team on your data transfer (within and outside of MSI), please fill out the [data transfer request form](https://docs.google.com/forms/d/e/1FAIpQLSd84tpEaXS4C9afAneGUGnW6dUtMhS1J9zunWgn5VFQjgRhYA/viewform)

The data transfer method will depend on how much data you are transferring (number of files and size), where you are transferring the data to, and what the data are (consider data sharing restrictions).

1. If the data is being sent to a collaborator at another institution, make sure you know who the collaborator is and what method is being used to share the data. Some of DCAN Labs' more frequent collaborators include DEAP, INDI, and UPenn.

2. If you are trying to upload a small testing dataset (less than 10 subject-session pairs), you will most likely be uploading to Box. Box is a HIPPA compliant secure storage service that is available across many institutions. Make sure you have already clarified where the data is going on Box. Box is **not recommended** for large amounts of data. UMN has a [set of Box guides.](https://it.umn.edu/services-technologies/self-help-guides/box-secure-storage-work-files-folders)

3. Use services like s3 sync, rsync, and Globus to sync data / upload data to the appropriate, specified locations. 

For information on how to use MSI to transfer/track data, see [the Data Storage page.](storage.md)

## S3 Transfers

For external transfers, instructions on setting up an .s3cfg and examples of transferring data using s3cmd can be found on the [s3 page.](s3.md)

## scp Transfers

This option is best for single folder or file transfers to Tier 1 from CMRR servers (e.g. NAXOS or Concierge). 

First you'll need to connect to the CMRR VPN via Cisco AnyConnect. 
    
1. On Cisco AnyConnect, select `UMN - Department Pools`
2. Select `AnyConnect-CMRR` as the Group
3. Enter your UMN x500 and password

Then you'll need to log into the CMRR system and find the data you need.

1. From a browser you can follow this [CMRR login SOP.](https://www.cmrr.umn.edu/computeruser/nc-cms/content/upload/C104-05%20-%20Connecting%20Macs%20to%20CMRR%20servers%20with%20NX%201.4.pdf) Or you can connect via a local terminal by running `ssh CMRRusername@range1.cmrr.umn.edu`
2. Navigate to the server where the data was pushed to
    - NAXOS path: `/home/naxos2-raid2/dicom`
    - Concierge path: `/home/range5-raid26/MIDB`
3. Find your scan data. They are typically named with this format: `dateofscan-STXXX-P`
    - Date of scan format is year-month-day

To transfer the data to MSI, you can run this command:

`scp -r /path/to/dicoms UMNx500@agate.msi.umn.edu:/path/to/dir/.`

- **NOTE:** Leave off the trailing slash from the dicom directory

- You can check the file count of the both directories to validate all of the expected data was transferred with the command `find /path/to/dicoms -type f | wc -l`

Note: Transfers are often more successful when transferring to your MSI home directory than a shared directory (e.g., `/home/faird/shared`). Once the data are in your home directory, you can move the data to a shared directory with a mv command. 

## Globus

Watch [this Globus tutorial](https://drive.google.com/file/d/1Yb_5L9pxIl0fquAtC83XeYleRl_uJo6S/view?usp=drive_link) for how to use Globus within a browser. 

To set up Globus on your local computer or a system without Globus already installed, follow the [instructions to install Globus connect personal](https://docs.globus.org/globus-connect-personal/) and log in using UMN credentials. Next, create a personal connect point. 

To transfer data to tier 2 storage (an s3 bucket), using the Globus file manager, choose **UMN MSI Tier2** on one side and your connect point on the other side. Find more information about [transferring to tier 2 storage](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-second-tier-storage-msi). If you are wanting to transfer data to tier 1 storage, choose **UMN MSI Home** as your location. MSI also has a page on how to [transfer data to/from MSI via Globus](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-msi-0).

Globus also has documentation on [using Globus via command line](https://docs.globus.org/cli/reference/).


For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).