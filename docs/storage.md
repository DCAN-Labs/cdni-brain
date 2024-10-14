
# Data Storage and Tracking

## Best Practices 
MSI is a shared use space, so it is important to know the approximate size impact your data (inputs and outputs) will have on the shared environment. When you plan to process a dataset, we recommend running jobs for a typical subject on Tier 1 and finding out how large the outputs are ( `du -sh --total /subject/output/path`). Multiply that size by the total number of subjects in your dataset and add the size of your inputs (`du -sh --total /dataset/input/path`) for the full impact. 

CDNI data are often used by multiple researchers and analysts. Please [document data locations](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit) to prevent 'double dipping' on storage space, especially when your work requires more than 1TB of space. For Tier 1 use, submit a storage [request form](https://docs.google.com/forms/d/e/1FAIpQLSd1QI_Hmi3khwITVctnaDJYY2M1NegsAWYPR6AXoodUCrrpZw/viewform?usp=sf_link)** about what kinds of data you will be putting onto Tier 1 storage and why Tier 1 is needed, specifically.

## Data storage options:

* Tier 1 space is limited to 150GB - 20TB per group (depending on the group's allocation). To see what your group allocation is, copy the link `https://www-archive.msi.umn.edu/group/<group>/storage`. into your browser bar with the group name in the path.
    
* [**S3 (aka Tier 2)](s3.md) storage should be used whenever possible.** New MSI users are limited to 5GB of Tier 2 storage. PIs are allocated 120TB of Tier 2 storage. To see how much space is used in a particular bucket, enter `s3cmd du -H s3://<bucketname>`. Access to a S3 storage bucket can usually be granted by any user who already has access to that storage bucket. [Read here](s3.md) for more on S3 and buckets. 

## ABCC Data / NGDR 

Outputs using ABCD/ABCC data **CANNOT BE STORED ON FAIRD**. `faird` is not under control of the UMN ABCD designated user credentials (DUC), which is required to access ABCD data. 

The NGDR, or Neuroimaging and Genetic Data Resources, is a specialized storage system employing tier 1 storage with restricted access. It is designed for storing ABCC data before uploading it to the NDA. Access to specific data subsets, like the ABCD subdirectory, is granted only to users on the ABCC DUC. This ensures a high level of security and confidentiality in handling neuroimaging and genetic information.

## Data Tracking 

* For CDNI data being stored on Tier 1, documentation of the dataset description, dataset location, dataset size, the primary owner of the data, and the estimated start and end date for use of the necessary storage space should be entered [here](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=870411543). For further information on allowable data on each share, visit the “Shares” tab on teh same spreadsheet.
  
* For CDNI data being stored on s3, documentation of the bucket name, dataset description, dataset location, bucket owner, and users who has access to the bucket should be entered [here](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit). 

* For information on backup and versioning procedures, please refer to the [Backup and Versioning PowerPoint Presentation](https://docs.google.com/presentation/d/1UiXIvrsQNqVTgAtFib7Lvk2wOdcRSPTcUfG1zme4fqs/edit#slide=id.g149c53b2342_0_205).

    **TO-DO**: in the near future, include what pipeline the dataset was processed with, what version, and the amount of successes/fails and why onto [this spreadsheet](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=0).

## Transfering Data from MSI to Local Storage

Your UMN computer has a local storage space that you may want to use for working on specific files. You can transfer files between local and MSI Tier 1 systems via [winSCP](https://www.msi.umn.edu/support/faq/how-do-i-use-winscp-transfer-data) or [FileZilla](https://www.msi.umn.edu/support/faq/how-do-i-use-filezilla-transfer-data).


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).
