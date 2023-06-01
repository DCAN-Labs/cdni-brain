# Virtual Machine (Container Building)

DCAN Labs has access to a Virtual Machine (VM) for the purpose of building Docker application containers (Docker builds require root access, which is not permitted to general users on the main MSI clusters, thus the need for the VM). The process to sign up for access to MIDB's VM for use with container developing is as follows:

1. Reach out to Kim (kweldon@umn.edu) or Luci (lmoore@umn.edu) and indicate that you would like access to the container and indicate what project or projects that you will need it for
2. Read through the [documentation](https://docs.google.com/document/d/1w1g0kLSchPKvEI9pZIBmhavFd2Mq2-r82ozVaBuL9EI/edit) on how to use the VM appropriately put together by Audrey 
3. Request access from OIT by submitting a ticket to help@umn.edu. Here is a template for the email:

            Subject Line: Add user to VM: umii-midbig-dev-docker.oit.umn.edu
            Body: 
            Hello, 
            Can you please add this user, x500, to the VM: umii-midbig-dev-docker.oit.umn.edu? As is the case with all other users please give them SUDO privileges to the VM.
4. After you receive access, try the following test to ensure that your access is working
    * Run `ssh <x500>@umii-midbig-dev-docker.oit.umn.edu`
    * When prompted enter your UMN ID password and two-factor authenticate with DUO
    * If that all works try testing if you have "sudo" privileges by typing `sudo docker images`
    * If you get an output that looks like the attached picture, you are all set, if not let Tim know.

    ![vm_example](img/vm_example.png)

5. For troubleshooting issues, there is a tiered triage system
    * You or whomever you select as point is the point of contact for VM access
    * That same person and you handle minor troubleshooting
    * If issues continue to occur, you or the point of contact reach out to Tim (hendr522@umn.edu) so that he can reach out to Doug Finley (finl0023@umn.edu)

**Common Issues**

If you are running into problems with your space/storage

* Is your container too big? 
* Have you cleared out old builds?
* Does someone else have builds on the VM at this time?