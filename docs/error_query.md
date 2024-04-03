# Error Query

Error Query scripts were developed to promote best data management standards and practices. Error Query can be run on any computational effort that produces error log files. At minimum, Error Query scripts will output a list of subjects/sessions in a csv format that experience a prespecified error by the user. Below is a brief overview on how to use Error Query scripts.

## Preliminary Setup

When running Error Query, the user will specify specific strings to search for in error log files and the Error Query scripts will automatically parse through all the error log files, looking for the specified strings in each. These error strings are passed in as a json file. Below is an example of a json file that was used for ABCD BIDS Conversion error querying. 

[//]: # (TODO: add in example json code here when MSI is back up)

## Running Error Query

Error Query requires the following arguments: 
Error Query allows for the following optional arguments:
Please use `--help` for further explanation on arguments.
Here is an example command that was used on ABCD BIDS Conversion error logs:

[//]: # (TODO: add in example run command here when MSI is back up)

## Optional: Merging Error Query Outputs

After running Error Query, a combined output of all the individual CSVs is returned as "all_err_types.csv". In some cases, it may be relevant to combine multiple instances of this file, particularily if there are multiple working directories for your computational efforts. In that case, merging the "all_err_types.csv" files together can be done with Merge Error Query, which will return two CSVs: one is an all-emcompassing list of error information and the other is combined list of counts for similar errors across working directories. Here is an example run command for Merge Error Query, particularily for the "all_err_types.csv" of multiple ABCD BIDS Conversion Error Query output directories:

[//]: # (TODO: add in example run command here when MSI is back up, and script is finished)