# File-mapper

File mapper maps processed outputs into the BIDS format for the abcd-hcp and infant-abcd-bids pipelines. It is designed to help users copy, move, or symlink files from one directory to another using a JSON file as input. The JSON keys are the source of the files and the JSON values are the desired destination path. This program has been used to move important files from a large dataset and would be applicable in any BIDS datasets or other renaming/mapping utility. Another added benefit of file-mapper is that it trims down the amount of files if you delete the original source directory of file-mapper after running it.

1. File-mapper usage:

    1. File-mapper is located in: `/home/faird/shared/code/internal/utilities/file-mapper`

    2. Select an example mapping json file from the “examples” folder in this location or create your own
    
    3. Run with the following command: python3 `./file_mapper_script.py <selected json file> -a copy -sp [full output directory of a single subject down to /file] -dp [output dir] -t SUBJECT=[part after “sub-”],SESSION=[part after “ses-”],PIPELINE=[folder name (e.g. abcd-bids)]`

        1. `PIPELINE` refers to the directory your outputs are in which are inside the derivatives folder.

2. See the Example 2 [here](https://github.com/DCAN-Labs/file-mapper) for a more in-depth explanation.