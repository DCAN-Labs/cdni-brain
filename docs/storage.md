
# Data Storage and Tracking

## Best Practices

MSI provides multiple storage locations for different stages of research workflows, including home directories, project space, Global Scratch, and Tier 2 storage. Because available space and policies can change, use MSI's current storage documentation when deciding where data should live.

See the [MSI File Storage documentation](https://userdocs.msi.umn.edu/storage/storage.html) for current storage types, allocations, retention policies, and recommended use cases.

When planning a CDNI processing workflow, it is still useful to estimate the total storage impact of your inputs and outputs before processing a full dataset. You can check the size of an input or output directory with:

```bash
du -sh --total /input/or/output/path
```

CDNI data are often used by multiple researchers and analysts. Please [document data locations](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit) to prevent 'double dipping' on storage space, especially when your work requires more than 1TB of space. For Tier 1 use, submit a storage [request form](https://docs.google.com/forms/d/e/1FAIpQLSd1QI_Hmi3khwITVctnaDJYY2M1NegsAWYPR6AXoodUCrrpZw/viewform?usp=sf_link) about what kinds of data you will be putting onto Tier 1 storage and why Tier 1 is needed, specifically.

## Data Storage Options

MSI provides several storage options that serve different purposes:

- **Tier 1 project space** is intended for active research data that needs high-performance filesystem access.
- **Global Scratch** is intended for temporary job data, staging, and intermediate outputs.
- **Tier 2** is intended for large-scale shared storage, inactive data, and S3-compatible workflows.

For current allocations, paths, quotas, snapshot availability, retention policies, and recommended uses for each storage type, see the [MSI File Storage documentation](https://userdocs.msi.umn.edu/storage/storage.html).

For CDNI-specific Tier 2 and S3 workflows, see our [S3 documentation](s3.md).

<div class="admonition attention">
    <p class="first admonition-title">Attention</p>
    <p class="last">
        Outputs using ABCD/ABCC data CANNOT BE STORED ON TIER 1. These spaces are not under control of the UMN ABCD designated user credentials (DUC), which is required to access ABCD data. All ABCD outputs should either be stored on the `cdni-nih-bdc` share, or processed in tmp space and synced to the s3.
    </p>
</div>

## Data Tracking 

* For CDNI data being stored on Tier 1, documentation of the dataset description, dataset location, dataset size, the primary owner of the data, and the estimated start and end date for use of the necessary storage space should be entered in the [Tier 1 Tab of our Data Location Google Sheet](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=870411543). For further information on allowable data on each share, visit the “Shares” tab on the same spreadsheet.
  
* For CDNI data being stored on s3, documentation of the bucket name, dataset description, dataset location, bucket owner, and users who have access to the bucket should be entered in the [Tier 2 tab of our Data Location Google Sheet](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit). It would be helpful to include what pipeline the dataset was processed with and what version in the dataset description.

* For information on backup and versioning procedures, please refer to the [Backup and Versioning PowerPoint Presentation](https://docs.google.com/presentation/d/1UiXIvrsQNqVTgAtFib7Lvk2wOdcRSPTcUfG1zme4fqs/edit#slide=id.g149c53b2342_0_205).

## Transferring Data Between MSI and Local Storage

MSI supports several methods for transferring data between local computers and MSI storage.

For Tier 1, common options include SFTP and Globus. For larger or long-running transfers, Globus is generally preferred because it can manage retries and continue transfers without requiring an active browser session.

For current transfer methods and step-by-step instructions, see [Transferring Data To and From MSI](https://userdocs.msi.umn.edu/storage/transferring_data.html).

For Globus-specific setup and sharing guidance, see the [MSI Globus documentation](https://userdocs.msi.umn.edu/storage/globus.html).


For questions, suggestions, or to note any errors, [post a Github issue](https://github.com/DCAN-Labs/cdni-brain/issues).
