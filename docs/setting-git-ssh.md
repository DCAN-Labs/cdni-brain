## Setting up GitHub SSH on Your Local Computer and MSI

Setting up GitHub SSH on your local computer and on MSI (Minnesota Supercomputing Institute) involves a series of steps. Below are the general steps for both environments:

### Open Terminal or Command Prompt:

On Windows, you can use Git Bash or PowerShell.
On macOS and Linux, use the terminal.
### Generate SSH Key:


```
ssh-keygen
```
Press Enter to accept the default file location and optionally set a passphrase.

### Copy SSH Public Key:

```cat ~/.ssh/id_rsa.pub | pbcopy   # macOS
   cat ~/.ssh/id_rsa.pub | clip     # Windows
```

### Add SSH Key to GitHub:

Open GitHub, go to Settings > SSH and GPG keys, and add a new SSH key. Paste the copied key.

## Setting up GitHub SSH on MSI:
### Connect to MSI:
Access the MSI cluster using your university credentials.

### Open Terminal:

Use the terminal or command-line interface on the MSI cluster.
### Generate SSH Key Pair:

```
ssh-keygen
```

Press Enter to accept the default file location and optionally set a passphrase.

### Copy SSH Public Key:

```
cat ~/.ssh/id_rsa.pub
```

### Add SSH Key to GitHub:

Open your GitHub account settings in a web browser.
Navigate to "SSH and GPG keys."
Add a new SSH key and paste the copied public key.
Save the new key.

### Testing the Connection:
In the MSI terminal, test the SSH connection to GitHub:

```
ssh -T git@github.com
```

By following these steps, you establish a secure and authenticated connection between your local computer and the MSI cluster, enabling seamless collaboration and version control with Git and GitHub. Remember to regularly update and manage your SSH keys for optimal security.