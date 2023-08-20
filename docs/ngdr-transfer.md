# How to Transfer ABCC Data to the NGDR

-   Create a "hashtable" text file of the data to be transferred, including a path of where the data is now to where we want the data to be. Two example lines of a previous "hashtable" are provided below.
```shell 
s3://ABCC_year2/derivatives/abcd-hcp-pipeline/sub-NDARINVXXXXXXXX/ses-2YearFollowUpYArm1/ /spaces/ngdr/ref-data/abcd/nda-3165-2020-09/derivatives/abcd-hcp-pipeline/sub-NDARINVXXXXXXXX/ses-2YearFollowUpYArm1/
s3://ABCC_year2/derivatives/abcd-hcp-pipeline/sub-NDARINVYYYYYYYY/ses-2YearFollowUpYArm1/ /spaces/ngdr/ref-data/abcd/nda-3165-2020-09/derivatives/abcd-hcp-pipeline/sub-NDARINVYYYYYYYY/ses-2YearFollowUpYArm1/
```

-   Upload the hashtable to [this box folder](https://umn.app.box.com/folder/180135363134) in "to_transfer"
-   Send an email to the MSI data management `data@msi.umn.edu` with a brief description of the data being transferred (to be able to find the request in your email) Ex: ABCC year 2 BIDS inputs , containing the following description: (cc Audrey Houghton, Jacob Lundquist, and Eric Feczko on the request)

`Hi all,
I need assistance from the abcd data manager, Kimberleigh Breen. I have a list of ABCD data in an s3 bucket that needs to be moved down to the NGDR. The abcd data manager already has access to the bucket. The paths that the data is currently at on the s3 are given with a corresponding path to send it to on the NGDR within text files. The text file is stored here within the "to_transfer" folder with access granted to the abcd data manager under the name {insert name of your hashtable file}. {provide any additional info that's unique to the data you are transferring}
Let me know if you need any additional information. Thanks,`

-   Follow-up with MSI weekly/bi-weekly to ensure the data gets transferred.
-   After MSI (Kimberleigh Breen or Naomi Hospodarsky-Sutherland) confirms that the data has been transferred, check that the permissions on a few of the data paths match the data that was already on the NGDR. If the data permissions aren't correct, ask for them to be corrected in the same email thread with the data transfer request and verify that the permissions have been fixed once they do so.
-   Move the hashtable from "to_transfer" to "complete" in [this box folder](https://umn.account.box.com/login?redirect_url=%2Ffolder%2F180135363134).
