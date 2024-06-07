# Common Roadblocks on MSI

38. If you are experiencing errors/issues with general usage or Slurm:

    * Contact the MSI help desk (help@msi.umn.edu)

31. If you get a `Disk quota exceeded` error, try changing your group:

    * First check that the share isn't full: `groupquota`

    * One file: `chgrp faird filename`

    * All files in the current directory: `chgrp faird *`
    
    * All files in a specific directory: `chgrp faird /home/faird/shared/*`

32. If you cannot access MSI or it's running slow:

    * Is it the first Wednesday of the month? MSI will be down all day

    * Is it the first week of the month? MSI can run slow during the week of maintenance day

    * Check the status of MSI [here](https://status.msi.umn.edu/)

    * Check the status of MSI nodes [here](https://download.genomics.umn.edu/website/slurmnodes/index.html)

33. If you get this error when trying to use Firefox:
            
            "Firefox is already running, but is not responding. To use Firefox, you must first close the existing Firefox process, restart your device, or use a different profile."

    * In the terminal, run `firefox -P` and select default 

    * If issue persists, open the file with `chromium-browser html_file_to_open` 

34. If you are running into space issues:

    * Open a new terminal and type `ls -al` to see all hidden files 

    * Do you see a lot of ".proc-" files? Run `rm -r .proc-*` to delete all files that start with that prefix

    * Run `cd .apptainer/cache/blob/blobs` and clear out the `sha256` and `oci-tmp` folders (run `rm -rf *` inside the folders to delete all of their contents without deleting the folder itself)

35. If you get a "not enough memory" error when trying to grab a srun:

    * Must be ssh'd into mangi/mesabi/agate (`ssh -Y mangi/mesabi/agate`)

36. If someone can't read/write/change your folders/files:

    * Run `chmod gu+rwx path/to/file/or/dir -R` (`-R` only needed for directories)

    * This will open up read, write, and execute permissions for the file/directory

37. If you need to change your active group:

    * Run `newgrp faird` (or whichever group you need to be in)
    
    * This will let you make new files in the proper group

    * Read more about this [here](https://opensource.com/article/19/9/linux-chgrp-and-newgrp-commands#:~:text=The%20newgrp%20command%20allows%20a%20user%20to%20override,all%20files%20must%20have%20the%20same%20group%20ownership.)

38. If you are getting FileNotFound errors in paths using `panfs/roc/`:

    * MSI has moved our software library to a new location to migrate away from hardware that is being decommissioned

    * These are the old paths and their new counterparts 
    ```
    /soft	->  /common/software/install/migrated
    /panfs/roc/msisoft	->  /common/software/install/migrated
    /panfs/roc/soft/el6 ->  /common/software/install/migrated.softel6
    /panfs/roc/intel	->  /common/software/install/migrated.intel
    ```

    * Find out more information [here](https://www.msi.umn.edu/support/faq/how-do-i-update-my-workflow-after-software-library-migration)


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).