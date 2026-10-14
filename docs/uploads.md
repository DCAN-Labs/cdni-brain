# Data Uploads and Transfer

**DCAN Data Transfer Request**

If you require assistance or advice from the DCAN computing team on your data transfer (within and outside of MSI), please fill out the [data transfer request form](https://docs.google.com/forms/d/e/1FAIpQLSd84tpEaXS4C9afAneGUGnW6dUtMhS1J9zunWgn5VFQjgRhYA/viewform)

The data transfer method will depend on how much data you are transferring (number of files and size), where you are transferring the data to, and what the data are (consider data sharing restrictions).

1. If the data is being sent to a collaborator at another institution, make sure you know who the collaborator is and what method is being used to share the data. Some of DCAN Labs' more frequent collaborators include DEAP, INDI, and UPenn.

2. If you are trying to upload a small testing dataset (less than 10 subject-session pairs), you will most likely be uploading to Box. Box is a HIPPA compliant secure storage service that is available across many institutions. Make sure you have already clarified where the data is going on Box. Box is **not recommended** for large amounts of data. UMN has a [set of Box guides.](https://it.umn.edu/services-technologies/self-help-guides/box-secure-storage-work-files-folders)

3. Use services like s3 sync, rsync, and Globus to sync data / upload data to the appropriate, specified locations. 

For information on how to use MSI to transfer/track data, see [the Data Storage page.](storage.md)

## S3 Transfers

For current MSI guidance on Tier 2 storage and S3 access methods, see the [MSI File Storage documentation](https://userdocs.msi.umn.edu/storage/storage.html) and [Tier 2 Data Management](https://userdocs.msi.umn.edu/storage/tier-2-data-management.html).

## scp Transfers

This option is best for single folder or file transfers to Tier 1 from CMRR servers (e.g. NAXOS or Concierge). 

First you'll need to connect to the CMRR VPN via Cisco AnyConnect. 
    
1. On Cisco AnyConnect, select `UMN - Department Pools`
2. Select `AnyConnect-CMRR` as the Group
3. Enter your UMN x500 and password

Then you'll need to log into the CMRR system and find the data you need.

1. From a browser you can navigate to this website: https://login2.cmrr.umn.edu/nxwebplayercan and follow this [CMRR login SOP.](https://www.cmrr.umn.edu/intranet/computer_help/connecting_macs_to_cmrr_servers_with_nx_-_use_instead/) Or you can connect via a local terminal by running `ssh CMRRusername@range1.cmrr.umn.edu`
2. Navigate to the server where the data was pushed to
    - NAXOS path: `/home/naxos2-raid2/dicom`
    - Concierge path: `/home/range5-raid26/MIDB`
3. Find your scan data. They are typically named with this format: `dateofscan-STXXX-PI-SUBID`
    - Date of scan format is year-month-day
    - Generally the PI's last name and the subject/session ID are included in the folder name

To transfer the data to MSI, you can run this command:

`scp -r /path/to/dicoms UMNx500@agate.msi.umn.edu:/path/to/dir/.`

- **NOTE:** Leave off the trailing slash from the dicom directory

- You can check the file count of the both directories to validate all of the expected data was transferred with the command `find /path/to/dicoms -type f | wc -l`

For general MSI guidance on transferring files into Tier 1 storage, including home and project space, see [Transferring Data To and From MSI](https://userdocs.msi.umn.edu/storage/transferring_data.html).

## Globus

For current instructions on using Globus with MSI, including transfers between a local computer, Tier 1 (`UMN MSI Home`), and Tier 2 (`UMN MSI Tier2`), see the [MSI Globus documentation](https://userdocs.msi.umn.edu/storage/globus.html).

For guidance on choosing between Globus, SFTP, `rclone`, and other MSI-supported transfer methods, see [Transferring Data To and From MSI](https://userdocs.msi.umn.edu/storage/transferring_data.html).


For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
