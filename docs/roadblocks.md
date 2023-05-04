# Common Roadblocks on MSI

31. If you get an error (e.g. about disk space), change your group:

    * One file: `chgrp faird filename`

    * All files in the current directory: `chgrp faird *`
    
    * All files in a specific directory: `chgrp faird /home/faird/shared/*`

32. If you cannot access MSI or it's running slow:

    * Is it the first Wednesday of the month? MSI will be down all day

    * Is it the first week of the month? MSI can run slow during the week of maintenance day

    * Check the status of MSI [here](https://status.msi.umn.edu/)