# Common Roadblocks on MSI

31. If you get an error (e.g. about disk space), change your group:

    * One file: `chgrp faird filename`

    * All files in the current directory: `chgrp faird *`
    
    * All files in a specific directory: `chgrp faird /home/faird/shared/*`

32. If you cannot access MSI or it's running slow:

    * Is it the first Wednesday of the month? MSI will be down all day

    * Is it the first week of the month? MSI can run slow during the week of maintenance day

    * Check the status of MSI [here](https://status.msi.umn.edu/)

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

## Other Issues 

37. If you need help with data processing:

    * Fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLSefH4cc3PKu2mAjIVAbWarhxyuJQKOq9gudpMO8_hT338En2Q/viewform) to request that the DCAN computing team process data for you

38. If you don't have access to needed data:

    * Fill out [this form](https://docs.google.com/forms/d/e/1FAIpQLSdRCr8vl0wTWgMWgxjCPSLIgDRVuatU_D5nvZ5OR3NmY-MUPA/viewform) to request access to data on MSI

39. If there are issues with this handbook or you have questions:

    * Post an issue [on Github](https://github.com/DCAN-Labs/data-processing-handbook)