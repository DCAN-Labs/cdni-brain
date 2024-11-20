# Workbench

Connectome Workbench is an open source visualization and discovery tool used to view and manipulate neuroimaging data. To use on MSI, you'll have to [load the workbench module](modules.md). 

There are two primary commands for workbench `wb_view` and `wb_command`.

- `wb_view` will open an interactive GUI used for inspecting images. This is often used during troubleshooting and QC of processing.

    * This GUI can be super slow as it requires a lot of resources to run. For a chance to increase performance speed, grab an srun with more memory. 

- `wb_command` has a collection of commands used to print out file information, perform calculations, merge images, and more manipulations. 


## Viewing Images

wb_view can be used to visually inspect many data types, including:

- CIFTI data (dlabel, dscalar, dtseries, pconn, etc)
- Surface data (.surf.gii files)
- Volume data (.nii/.nii.gz files)


## Manipulating data

Here are some useful commands that are called with `wb_command <command> <options>`. To see the full usage of any command, run `wb_command <command>` with no inputs. Running `wb_command -list-commands` will show all of the possible workbench commands and a brief description of their function. 

`wb_command -file-information /path/to/file`

- This command will list information about the contents of a file, like its dimensions.

`wb_command -cifti-separate /path/to/cifit_file direction`

- This command separates a CIFTI into separate GIFTIs, which can be helpful for functions that only work on GIFTIs
