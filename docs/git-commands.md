# Common Git Commands

This page contains common commands used to work with Github repositories via git. This is by no means an exhaustive list of commands. 

## Basics 

`git clone https://url-to-repo OR git@github.com:repo/location.git`

- Clone a github repo. Two methods: HTTPS or SSH. SSH is recommended as it will not require a token every time you interact with the repo (push/pull/etc) on MSI. Learn how to set up SSH with Github [on this page](github-quick-guide.md). If you do use HTTPS on MSI you will need to [create a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic) and enter it every time you interact with the repo. 
- If you would like to only clone a specific branch, add `-b branch_name --single-branch` after `clone`.

`git status`

- Check the status of the repo you're working on. This will tell you what changes have been made and what branch you're on.

`git checkout -b branch_name`

- Move to a new/different github branch. If the branch already exists, do not include the `-b`. 

- `git branch new_branch_name` also creates a new branch but doesn't also move you to the new branch

`git add file_or_folder`

- Add changes from a specific file/folder to your commit. To add multiple files at once there are several options:

    * `-A` to add all new, modified, and deleted files

    * `-u` to add all modified and deleted files but not new files 

`git commit -m "commit message"`

- Commit a set of changes, make sure you have a descriptive message

`git push`

- Push your commit to the github repo

- If you made a new branch locally, to add it to the origin, run `git push --set-upstream origin branch-name`

`git pull`

- Pull the contents of the github repo to your local computer 

## Advanced

If your branch is out of date from the main branch:

```
git checkout main
git pull main
git checkout branch_to_update
git rebase main
```

If a branch name changed on Github but it still has the old name locally and you can no longer push to/pull from the old branch name:

```
git branch -m old_name new_name
git fetch origin
git branch -u origin/new_name new_name
git remote set-head origin -a
```


