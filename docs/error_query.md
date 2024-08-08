# Error Query

Error query scripts were developed to promote best data management standards and practices. Error query can be run on any computational effort that produces error log files. At minimum, these scripts will output a list of subjects/sessions in a csv format that experience a prespecified error by the user. Below is a brief overview on how to use the scripts.

## Preliminary Setup

When running error query, the user will specify specific strings to search for in error log files and the error query scripts will automatically parse through all the error log files, looking for the specified strings in each. These error strings are passed in as a json file. Below is an example of a json file that was used for ABCD-BIDS error querying. Note that to save space, this is **not the entire json** that was used. The key is what the output csv will be named and the value is the error message that it is searching for.

```
{
    "prefreesurfer":"Exception: error caught during stage: PreFreeSurfer",
    "freesurfer": "Exception: error caught during stage: FreeSurfer",
    "postfreesurfer": "Exception: error caught during stage: PostFreeSurfer",
    "fmrivolume": "Exception: error caught during stage: FMRIVolume",
    "fmrisurface": "Exception: error caught during stage: FMRISurface",
    "dcanboldprocessing": "Exception: error caught during stage: DCANBOLDProcessing",
    "executivesummary": "Exception: error caught during stage: ExecutiveSummary",
    "customclean": "Exception: error caught during stage: CustomClean",  
    "time_limit": "DUE TO TIME LIMIT",
    "oom": "oom-kill",
    "assertion_error": "AssertionError",
    "key_error": "KeyError",
    "index_error": "IndexError",
}
```

## Running Error Query

Note that if an error log has multiple errors, the subject will be added to the list of the first error encountered in the log. 

You can find the error query code on MSI at `/home/faird/shared/code/internal/utilities/MSI-utilities/error_query/`

`new_error_query.py` is the most up to date script and requires a single argument: `-l /path/to/output/logs`

It also allows for the following optional arguments:

- `-o /path/to/output/folder/`
- `-e /path/to/errors.json`
- `r /path/to/run/files/`
- `-s /path/to/subjectIDs_to_search_for.txt`
- `-del` to delete old output/error logs 
- `-p` to include path of error log in output csv files

Please use `new_error_query.py --help` for further explanation on arguments.

Here is an example command that was used on ABCD BIDS Conversion error logs:

`python3 new_error_query.py -l /path/to/output_logs/ -o /path/to/error_query_results/ -e err_strings.json -del`

## Optional: Merging Error Query Outputs

After running the error query, a combined output of all the individual CSVs is returned as "all_err_types.csv". In some cases, it may be relevant to combine multiple instances of this file, particularily if there are multiple working directories for your computational efforts. In that case, merging the "all_err_types.csv" files together can be done with `merge_error_outputs.py`, which will return two CSVs: one is an all-emcompassing list of error information and the other is combined list of counts for similar errors across working directories. Here is an example run command for `merge_error_outputs.py`, particularily for the "all_err_types.csv" of multiple error query output directories:

`python merge_error_outputs.py /path/to/error/query/results/directory1/ /path/to/error/query/results/directory2/ /path/to/merge/output/dir/`