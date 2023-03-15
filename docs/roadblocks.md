# Common Roadblocks on MSI

31. If you get an error (e.g. about disk space), change your group:

    70. One file: `chgrp faird filename`

    71. All files in the current directory: `chgrp faird *`
    
    72. All files in a specific directory: `chgrp faird /home/faird/shared/*`