# A Beginner's Guide to Using GitHub for the First Time

GitHub is a web-based platform that uses Git for version control, enabling developers to collaborate on projects. It provides a centralized space for managing code changes and supports key concepts like version control, repositories, branching, and pull requests. 

**Version Control:** Enables multiple developers to work concurrently, tracking changes and allowing reverting to previous versions.

**Repositories:** Storage for project assets, public or private, where code and documentation are kept.

**Branching:** Developers create branches to work on features separately, merging them back into the main branch.

**Pull Requests:** Proposed changes submitted for review and discussion before merging into the main codebase.

**Collaboration:** GitHub offers tools like Issues, Discussions, and Actions for task tracking, communication, and automation.

## Benefits of GitHub

**Collaboration**: 
Facilitates collaboration among developers, whether local or global.

**Version Control**: 
Manages code changes over time, enhancing tracking and reverting capabilities.
Community and Open Source: Hub for open-source projects, allowing global contributions and collaboration.

**Project Management**: 
Supports features like Issues and Projects for tracking tasks and discussions.

**Visibility**: 
Public repositories showcase work, build portfolios, and encourage open-source contributions.

## Setting up GitHub SSH Keys

Setting up GitHub SSH on your local computer and on MSI (Minnesota Supercomputing Institute) involves a series of steps. Below are the general steps for both environments:

**Open Terminal or Command Prompt:**

On Windows, you can use Git Bash or PowerShell.<br>
On macOS and Linux, use the terminal.<br>
If setting up on MSI, first connect to a MSI cluster then open a terminal. 

**Generate and copy SSH Key:**

```
ssh-keygen
# Press Enter to accept the default file location and optionally set a passphrase.
cat ~/.ssh/id_rsa.pub | pbcopy   # macOS
cat ~/.ssh/id_rsa.pub | clip     # Windows
# Or just open the file and copy the contents. 
```

**Add SSH Key to GitHub:**

Open GitHub, go to Settings > SSH and GPG keys, and add a new SSH key. Paste the copied key and save it. 

**Testing the Connection on MSI:**

In the MSI terminal, test the SSH connection to GitHub:

```
ssh -T git@github.com
```

By following these steps, you establish a secure and authenticated connection between your local computer and the MSI cluster, enabling seamless collaboration and version control with Git and GitHub. Remember to regularly update and manage your SSH keys for optimal security.

## Creating a GitHub Repository

To see more commonly used github commands with more description, see [this page.](git-commands.md)

**Sign up for GitHub:**

Create an account at [github.com](https://github.com/).

**Create a Repository:**

Click "+" in the top right, select "New repository," fill in details, and click "Create repository."

**Clone the Repository:**

On the GitHub page of your repository, select the green Code button and select HTTPS or SSH then click the double square icon to copy the URL to your clipboard.
Open the terminal on your local computer where you want to clone the repository and run the following command to clone the repository:

```
git clone <repository_url>
```

If you're on Windows and want to use a Unix-like terminal for Git operations, you can install Git Bash. Git Bash provides a command-line interface with Git functionalities.

**Make Changes and Collaborate:**

Modify files in the local repository.
Create branches, commit changes, and push them to GitHub.

```
# Check the status of your changes
git status

# Add changes to the staging area
git add .

# Commit changes with a descriptive message
git commit -m "Your commit message"
```

**Create Branches, Commit Changes, and Push to GitHub:**

```
git branch your-branch-name     # Create a new branch for your feature or fix
git checkout your-branch-name   # Switch to the new branch

# Make and commit changes on the new branch
git add .
git commit -m "Implementing a new feature"

# Push changes to GitHub
git push origin your-branch-name
```

**Create a Pull Request and Merge Changes:**

On GitHub navigate to your repository. Go to the "Pull Requests" tab. Select the pull request you want to merge. Click "Merge pull request."
More information on GitHub can be found on [Git Documentation website.](https://git-scm.com/doc)


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).