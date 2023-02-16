# Storage Allocation Estimates

Figure out the approximate average storage impact your jobs (inputs and outputs) will have and estimate the total storage impact. For processing and analyzing a sample of individual subjects, run 3-5 different subjects’ jobs on Tier 1 and take the largest output and multiply it by the total number of subjects: du -sh --total /subject/output/path

16. Make the decision of where the processing will take place

    - **S3 storage should be used whenever you can.** Data is organized by your personal bucket, and you must manage who has permissions and access to your S3 bucket. Every user is allocated 120TB, so space is less of a concern for Tier 2 storage. Size estimates are important for the S3 because you might not have enough space left over in your allocation on the S3. **When you make an S3 bucket, please document the data locations on the [DCAN Lab Data Locations & Processing Planning doc](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit) on the Tier 2 tab.**

    - Tier 1 is for testing. **When you will be doing a large Tier 1 processing job (> 1TB), please put your processing plans on the [DCAN Lab Data Locations & Processing Planning doc](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit) on the Tier 1 tab AND submit a [request form](https://docs.google.com/forms/d/e/1FAIpQLSd1QI_Hmi3khwITVctnaDJYY2M1NegsAWYPR6AXoodUCrrpZw/viewform?usp=sf_link)** about what kinds of data you will be putting onto Tier 1 storage and why Tier 1 is needed, specifically.

17. Put the input data where it needs to be. This will more than likely always be on the S3.

    - If on the S3, make a bucket and give permissions to the appropriate individuals (see section [here](s3.md)), and sync the data there.

    - If on tier 1, fill out the request form (if above 1 TB).