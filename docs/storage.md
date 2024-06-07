# Storage Allocation Estimates

Figure out the approximate average storage impact your jobs (inputs and outputs) will have and estimate the total storage impact. For processing and analyzing a sample of individual subjects, run 3-5 different subjects’ jobs on Tier 1 and take the largest output and multiply it by the total number of subjects: `du -sh --total /subject/output/path`

16. Make the decision of where the processing will take place

    - **S3 storage should be used whenever you can.** Data is organized by your personal bucket, and you must manage who has permissions and access to your S3 bucket. Every user is allocated 120TB, so space is less of a concern for Tier 2 storage. Size estimates are important for the S3 because you might not have enough space left over in your allocation on the S3. **When you make an S3 bucket, please document the data locations on the [DCAN Lab Data Locations & Processing Planning doc](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit) on the Tier 2 tab.**

    - Tier 1 is for testing. **When you will be doing a large Tier 1 processing job (> 1TB), please put your processing plans on the [DCAN Lab Data Locations & Processing Planning doc](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit) on the Tier 1 tab AND submit a [request form](https://docs.google.com/forms/d/e/1FAIpQLSd1QI_Hmi3khwITVctnaDJYY2M1NegsAWYPR6AXoodUCrrpZw/viewform?usp=sf_link)** about what kinds of data you will be putting onto Tier 1 storage and why Tier 1 is needed, specifically.

17. Put the input data where it needs to be. This will more than likely always be on the S3.

    - If on the S3, make a bucket and give permissions to the appropriate individuals (see section [here](s3.md)), and sync the data there.

    - If on tier 1, fill out the [request form](https://docs.google.com/forms/d/e/1FAIpQLSd1QI_Hmi3khwITVctnaDJYY2M1NegsAWYPR6AXoodUCrrpZw/viewform?usp=sf_link) (if above 1 TB). Choose your share based on the `groupquota` of all shares (`groupquota -g share_name`). Do not put anything related to ABCD on the `faird` share.

18. If you are producing outputs using ABCD/ABCC data, **IT CANNOT BE STORED ON FAIRD** due to this share not being under control of the UMN DUC, which is required to access ABCD data. 

## Data Tracking 

* For data being uploaded to tier 1 storage space, document a description of the dataset and a path to where the data lives, an estimate on the amount of space needed to store the data, the share it’s been uploaded to, the primary owner of the data, and the estimated start and end date for use of the necessary storage space to [this spreadsheet](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=870411543). For further information on allowable data on each share, visit the “Shares” tab on [the above spreadsheet here](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=2135306680).
* For the data being uploaded on the s3, document a description of the data and a path to where the data lives, the project it's associated with, the owner of the s3 bucket, and everyone else who has access to the bucket onto [this spreadsheet](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit). 

* For information on backup and versioning procedures, please refer to the [Backup and Versioning PowerPoint Presentation](https://docs.google.com/presentation/d/1UiXIvrsQNqVTgAtFib7Lvk2wOdcRSPTcUfG1zme4fqs/edit#slide=id.g149c53b2342_0_205).

    **TO-DO**: in the near future, include what pipeline the dataset was processed with, what version, and the amount of successes/fails and why onto [this spreadsheet](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=0).

## Transfering Data from MSI to Local Storage

Your UMN computer has a local storage space that you may want to use for working on specific files. You can transfer files between local and MSI Tier 1 systems via [winSCP](https://www.msi.umn.edu/support/faq/how-do-i-use-winscp-transfer-data) or [FileZilla](https://www.msi.umn.edu/support/faq/how-do-i-use-filezilla-transfer-data).


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).