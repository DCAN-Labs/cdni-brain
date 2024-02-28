# Data Collections on the NDA

## DCAN Labs ABCD-BIDS Community Collection (ABCC) - C3165

The ABCD-BIDS Community Collection (ABCC) from the Developmental Cognition and Neuroimaging (DCAN) Labs contains a regularly updated dataset of ABCD Brain Imaging Data Structure (BIDS) version 1.2.0 pipeline inputs and derivatives. Source data passed initial acquisition quality control from the ABCD Data Analysis and Informatics Resource Center (DAIRC) and were processed by DCAN Labs. The input DICOM data to this BIDS version 1.2.0 (https://www.nature.com/articles/sdata201644) data collection were retrieved from the NIMH Data Archive (NDA) share of ABCD fast-track data (https://nda.nih.gov/edit_collection.html?id=2573) BIDS input data were converted from DICOMs using Dcm2Bids (https://github.com/cbedetti/Dcm2Bids). BIDS derivatives data were derived from the DCAN Labs ABCD-BIDS MRI processing pipeline which outputs Human Connectome Project (HCP) Minimal Preprocessing Pipelines-style data in both volume and surface spaces (https://doi.org/10.5281/zenodo.2587210, https://doi.org/10.1016/j.neuroimage.2013.04.127). This collection is independent from ABCD Data Collection 2573. Users may access ABCD DICOM files via the ABCD fast-track imaging data release in Collection 2573. Read the live online documentation for details about data currently available at https://collection3165.readthedocs.io.

**Note**: This is where we upload data to and where the [downloader](https://github.com/DCAN-Labs/nda-abcd-s3-downloader) is meant to pull data from

## Adolescent Brain Cognitive Development (ABCD) - C2573

The ABCD Study is designed to permit the scientific community to answer important questions about the relationships among physical and mental health, cognition, substance use (SU), culture and environment, genetics, environmental exposures and brain development of adolescents. The ABCD Study is a nationwide study of more than 10,000 9-10 year-olds conducted at 21 sites (29% of the US population lives within 50 miles of our geographically spread sites), that, uniquely, can provide a representative sample and a large twin sample that together can help distinguish environmental, sociocultural, and genetic factors relevant to adolescent health and brain development. We ensure cohesion and standardization with a recruitment strategy designed by a professional survey company (experience with Monitoring the Future), standardized environmental, neurocognitive and mental health assessments, MRI assessments with scanners using modified harmonized Human Connectome Project procedures, and computerized data collection with real-time quality control. Developmentally tailored assessments will have stable sensitivity and construct validity across childhood and adolescence, capture even subtle changes in SU, mental health, neurocognition, development, and environment, and employ novel bioassays and passive data collection from mobile devices. The retention plan builds on the experience of our investigators to ensure success.

**Note**: This is where the DICOM data, the fast track qc, and the behavioral data comes from and is where [abcd-dicom2bids](https://github.com/DCAN-Labs/abcd-dicom2bids) is meant to pull data from.

##  Downloading and Validating Previous Submissions on NDA

To validate what was uploaded on NDA and ensure the accuracy of data collection follow these steps:

- Ensure you have 'Admin' permissions on your NDA Collection.
- Visit your NDA Collection and navigate to the 'Submissions' tab.
- Select the submission(s) you wish to review and download.
- Click "Add to Filter Cart" located in the bottom left corner of the page.
- Wait for your Filter Cart to finish updating.
- Click “Create Data Package/Add Data to Study”.
- Create your data package, ensuring to include all relevant files for validation.
- Download your submission using the [NDA Download Manager](https://nda.nih.gov/nda/nda-tools#download-manager) or the Python tool.

By following these steps, you can not only download previous submissions from NDA but also ensure that the data you upload to NDA are validated and accurately reflected in your collection.


## Request to Archive Data on the NDA

### Detailed Reasoning for Archival:

Provide a detailed explanation outlining the specific reasons for archiving the data.

### Submission ID(s) for Archival:

Identify and list the Submission ID(s) you intend to archive. This information is crucial for accurately categorizing and preserving the relevant data. You can also find the IDs by following the steps below:

- Navigate to the Collections tab on the platform.

- Select the specific collection from which you want to archive data.

- Within the selected collection, click on the Submissions tab.

- Locate the ID column associated with each submission. 

### NDA Collection Number:

Specify the NDA Collection number. This detail is necessary for organizing and associating the archived data with the correct collection.

### CC to PI and Program Officer:

In addition to the archival request, please ensure to copy your Principal Investigator (PI) and the Program Officer associated with the Collection. 
