# Data Uploads and Transfer

## Upload Documentation

* For data being uploaded to tier 1 storage space, document a description of the dataset and a path to where the data lives, an estimate on the amount of space needed to store the data, the share it’s been uploaded to, the primary owner of the data, and the estimated start and end date for use of the necessary storage space to [this spreadsheet](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=870411543). For further information on allowable data on each share, visit the “Shares” tab on [the above spreadsheet here](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=2135306680).
* For the data being uploaded on the s3, document a description of the data and a path to where the data lives, the project it's associated with, the owner of the s3 bucket, and everyone else who has access to the bucket onto [this spreadsheet](https://docs.google.com/spreadsheets/u/0/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit). 

    **TO-DO**: in the near future, include what pipeline the dataset was processed with, what version, and the amount of successes/fails and why onto [this spreadsheet](https://docs.google.com/spreadsheets/d/1QpKYJQqhuxoQhErBscAEev9npsd1RgKS8KdCL6FiuEo/edit#gid=0).

## Data Upload and Transfer Steps

After data has been fully processed, it's important to ask yourself these questions again: where is the data going, and who cares about it?

28. If it's ABCD data, it is most likely going to be an [NDA upload](https://github.com/DCAN-Labs/nda-bids-upload). 

29. If the data is being sent to a collaborator, make sure you know who the collaborator is and what method is being used to share the data. Some of DCAN Labs' more frequent collaborators include DEAP, INDI, and UPenn.

30. If you are trying to upload a small testing dataset (less than 10 subject-session pairs), you will most likely be uploading to Box. Make sure you have already clarified where the data is going on Box.

Use services like s3 sync, rsync, and globus to sync data / upload data to the appropriate locations. You generally want to use the easiest method for transferring data. Instructions on setting up an .s3cfg are as follows:

1. Create a specific `.s3cfg-RANDOM` file somewhere in your home directory. You can name it whatever you want, except "`.s3cfg`". I recommend naming it something easily identifiable for you.

2. Run this command and give the _Access Key ID_ and _Secret Access Key_ when prompted. You should not have to fill in any other fields unless it is hosted by something other than _s3.amazonaws.com_: `s3cmd -c ~/.s3cfg-RANDOM --configure`                                                Note: the `.s3cfg-RANDOM` here is the one you made in step one.

3. Test your access through an `ls` command - the `s3cmd -c ~/.s3cfg-RANDOM` will have to be part of every command you run to access the buckets that require the credentials you provided.

## Globus

(globus tutorial video isnt uploaded correctly on google drive)


Using [this link](https://www.globus.org/), follow the instructions to install globus connect personal and log in using UMN credentials. Next, create a personal connect point. To transfer data to tier 2 storage (an s3 bucket), using the globus file manager, choose UMN MSI Tier2 on one side and your connect point on the other side. If you are wanting to transfer data to tier 1 storage, choose UMN MSI Home as your location.


**Note**: if you want to upload data from an external hard drive, go to preferences/access and add the hard drive. Visit [this link](https://www.google.com/url?q=https://blogs.iu.edu/ncgas/2021/10/19/adding-an-external-hardrive-to-your-iu-globus-account/&sa=D&source=docs&ust=1667391250182672&usg=AOvVaw3TAVihC6pc10bY2FObWl-4) for a more detailed step-by-step process. 

## DCAN Data Transfer

If you would like to transfer data to/from/between the UMN MSI servers and would like help from the DCAN computing team, see [this page](requests.md)