# Virtual Machine (Container Building)

DCAN Labs has access to a Virtual Machine (VM) for the purpose of building Docker application containers (Docker builds require root access, which is not permitted to general users on the main MSI clusters, thus the need for the VM). The process to sign up for access to MIDB's VM for use with container developing is as follows:

1. Reach out to Tim, hendr522@umn.edu that you would like access to the container and indicate what project or projects that you will need it for
2. Read through the [documentation ](https://docs.google.com/document/d/1w1g0kLSchPKvEI9pZIBmhavFd2Mq2-r82ozVaBuL9EI/edit)on how to use the VM appropriately put together by Audrey 
3. Tim will submit a ticket to OIT on your behalf. Once you hear back from Tim that you do have access try to attempt the following test below to ensure that your access is working
    1. Run ssh &lt;x500>@umii-midbig-dev-docker.oit.umn.edu
    2. When prompted enter your UMN ID password and two-factor authenticate with DUO
    3. If that all works try testing if you have "sudo" privileges by typing sudo docker images
    4. If you get an output that looks like the attached picture, you are all set, if not let Tim know.

    ![vm_example](img/vm_example.png)