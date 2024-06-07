# Data Uploads and Transfer

## DCAN Data Transfer Request

If you require assistance or advice from the DCAN computing team on your data transfer (within and outside of MSI), please fill out the [data transfer request form](https://docs.google.com/forms/d/e/1FAIpQLSd84tpEaXS4C9afAneGUGnW6dUtMhS1J9zunWgn5VFQjgRhYA/viewform)

## Data Transfer Methods

The data transfer method will depend on how much data you are transferring (number of files and size), where you are transferring the data to, and what the data are (consider data sharing restrictions).

1. Transfers of ABCD data will most likely going include an [NDA upload](https://github.com/DCAN-Labs/nda-bids-upload).

2. If the data is being sent to a collaborator at another institution, make sure you know who the collaborator is and what method is being used to share the data. Some of DCAN Labs' more frequent collaborators include DEAP, INDI, and UPenn.

3. If you are trying to upload a small testing dataset (less than 10 subject-session pairs), you will most likely be uploading to Box. Box is a HIPPA compliant secure storage service that is available across many institutions. Make sure you have already clarified where the data is going on Box. Box is **not recommended** for large amounts of data. UMN has a [set of guides](https://it.umn.edu/services-technologies/self-help-guides/box-secure-storage-work-files-folders) on how to use Box.


Use services like s3 sync, rsync, and Globus to sync data / upload data to the appropriate, specified locations. 

For information on how to use MSI to transfer/track data, see [this page.](storage.md)

### S3 Transfers

For external transfers, instructions on setting up an .s3cfg can be found [here.](s3.md) For examples of transferring data using s3cmd, see [s3.](s3.md)

### scp Transfers

This option is best for single folder or file transfers to Tier 1. From your local computer or the source server (e.g., CMRR's servers), run the following command:

```
scp -o IdentitiesOnly=yes -r /path/to/data/folder umnID@mesabi.msi.umn.edu:/path/to/msi/target/umnID/.
```

We include `-o IdentitiesOnly=yes` to only reference the server related to your current ssh path (i.e Mesabi, Mangi, Agate).

Note: Transfers are often more successful when transferring to your MSI home directory than a shared directory (e.g., `/home/faird/shared`). Once the data are in your home directory, you can move the data to a shared directory with a mv command. 

### Globus

Watch [this tutorial](https://drive.google.com/file/d/1Yb_5L9pxIl0fquAtC83XeYleRl_uJo6S/view?usp=drive_link) for how to use Globus within a browser. 

To set up Globus on your local computer or a system without Globus already installed, using [this link](https://docs.globus.org/globus-connect-personal/), follow the instructions to install Globus connect personal and log in using UMN credentials. Next, create a personal connect point. 

To transfer data to tier 2 storage (an s3 bucket), using the Globus file manager, choose **UMN MSI Tier2** on one side and your connect point on the other side. Find more information about transferring to tier 2 storage [here](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-second-tier-storage-msi). If you are wanting to transfer data to tier 1 storage, choose **UMN MSI Home** as your location. More information about setting up Globus can be found [here](https://www.msi.umn.edu/support/faq/how-do-i-use-globus-transfer-data-msi-0).

For using Globus with command line, see [here](https://docs.globus.org/cli/reference/).

## NDA Uploads

The full instructions for performing an upload to the NDA can be found [here.](https://github.com/DCAN-Labs/nda-bids-upload). This section will merely provide guidelines for uploading data specifically from MSI within DCAN Labs. 

The upload working directories as well as the necessary jsons and yamls can be found at `/home/rando149/shared/projects/ABCD/ABCC_Upload/`

Create your own folder in the `working_directories` subfolder and name it to include the dataset, pipeline it was processed with, and the year of data you're uploading. An example of a name uploading baseline (year1) ABCC data that's been processed with the abcd-hcp-pipeline-v0.1.3 would be `ABCC_year1_v0.1.3_submission`

Examples of sbatch files for the prepare.py and upload.py scripts can be found under the [utilities folder on the upload repo](https://github.com/DCAN-Labs/nda-bids-upload/tree/main/utilities). 

prepare.py is not set up to work with pull data from the s3 so make sure your input data lives on tier1 (this will typically be the NGDR).

After you've run prepare.py, you'll need to make an `upload_directories.txt` file that contains the datatypes that you are uploading. This is needed for the upload.py script. 

When submitting the upload.py sbatch, it is important to submit them in arrays. The NDA servers can't handle too many submissions at once and will crash if they are overloaded. To prevent this, we upload batches of 500 files in groups of 5. The upload.py script is already set up to create batches of 500 files, so to submit 5 arrays, this line needs to be in your sbatch:

`#SBATCH --array=0-34%5` 

- This line will depend on how many datatypes you are uploading, the `34` in the example is the number of datatypes being uploaded.

- The `5` is how many arrays you're submitting at once. 5 has been chosen based on past uploads and communication with the NDA, who have indicated that they can (typically) guarantee that they have 5 different servers running. 

- This line will submit 5 jobs at a time and will wait for those to finish before submitting 5 more.


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).