# Common Git Commands

This page contains common commands used to work with Github repositories via git. This is by no means an exhaustive list of commands. 

## Basics 

`git clone url_to_repo`

- Clone a github repo. If you would like to only clone a specific branch, add `-b branch_name --single-branch` after `clone`.

`git status`

- Check the status of the repo you're working on. This will tell you what changes have been made and what branch you're on.

`git checkout -b new_branch_name`

- Move to a new github branch. If the branch already exists, do not include the `-b`. 

    * `git branch new_branch_name` also creates a new branch but doesn't also move you to the new branch

`git add file_or_folder`

- Add changes from a specific file/folder to your commit. To add multiple files at once there are several options:

    * `-A` to add all new, modified, and deleted files

    * `-u` to add all modified and deleted files but not new files 

`git commit -m "commit message"`

- Commit a set of changes, make sure you have a descriptive message

`git push`

- Push your commit to the github repo

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

If a branch name changed and you can no longer push to/pull from the old branch name:

```
git branch -m old_name new_name
git fetch origin
git branch -u origin/new_name new_name
git remote set-head origin -a
```


