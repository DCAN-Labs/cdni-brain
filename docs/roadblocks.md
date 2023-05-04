# Common Roadblocks on MSI

31. If you get an error (e.g. about disk space), change your group:

    * One file: `chgrp faird filename`

    * All files in the current directory: `chgrp faird *`
    
    * All files in a specific directory: `chgrp faird /home/faird/shared/*`

32. If you cannot access MSI or it's running slow:

    * Is it the first Wednesday of the month? MSI will be down all day

    * Is it the first week of the month? MSI can run slow during the week of maintenance day

    * Check the status of MSI [here](https://status.msi.umn.edu/)

33. If you get this error when trying to use Firefox "Firefox is already running, but is not responding. To use Firefox, you must first close the existing Firefox process, restart your device, or use a different profile."

    * In the terminal, run `firefox -P` and select default 

    * If issue persists, open the file with `chromium-browser html_file_to_open` 