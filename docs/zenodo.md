# ZENODO

To publish a GitHub repository on Zenodo, follow the steps outlined [HERE](https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content).

## How it works
Zenodo automatically imports metadata from your GitHub repository to fill out additional information such as authorship. To ensure that the metadata is captured correctly and nicely formatted, you can add a .zenodo.json file (see example for CABINET [here](https://github.com/DCAN-Labs/CABINET/blob/main/.zenodo.json)). Note that the formatting of this file is very particular, so it's best to copy and paste a working example as a starting point, otherwise the file may cause Zenodo to error when publishing a new release.

Note that if there is additional metadata other than what's contained in the .zenodo.json file that you manually edit within Zenodo, this information won't transfer to future releases, so if not all of the metadata you would like to include fits into the .zenodo.json file, it's likely better to not include this file at all.

Metadata can be updated without having to re-publish under a new DOI, but it has to be done manually using the Edit button on Zenodo as opposed to pulling new information from an updated .zenodo.json file:

## Editing the metadata of a published record
On the record page and click the orange Edit button in the top-right corner. You will be taken to a form similar to the upload form, where you will be able to edit almost all of the record’s metadata. Once you are done modifying it, click “Save” and then “Publish”." For more information visit [Zenodo help webpage](https://help.zenodo.org/).


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).