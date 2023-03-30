# Common Roadblocks on MSI

31. If you get an error (e.g. about disk space), change your group:

    * One file: `chgrp faird filename`

    * All files in the current directory: `chgrp faird *`
    
    * All files in a specific directory: `chgrp faird /home/faird/shared/*`