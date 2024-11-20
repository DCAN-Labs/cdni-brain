# Common Git Commands

This page contains common commands used to work with GitHub repositories via git. This is by no means an exhaustive list of commands. 

## Basics 

`git clone https://url-to-repo OR git@github.com:repo/location.git`

- Clone a GitHub repo. Two methods: HTTPS or SSH. SSH is recommended as it will not require a token every time you interact with the repo (push/pull/etc) on MSI. Learn how to set up SSH with GitHub [on this page](github-quick-guide.md). If you do use HTTPS on MSI you will need to [create a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic) and enter it every time you interact with the repo. 
- If you would like to only clone a specific branch, add `-b branch_name --single-branch` after `clone`.

`git status`

- Check the status of the repo you're working on. This will tell you what changes have been made and what branch you're on.

`git checkout -b branch_name`

- Move to a new/different GitHub branch. If the branch already exists, do not include the `-b`. 

- `git branch new_branch_name` also creates a new branch but doesn't also move you to the new branch

`git add file_or_folder`

- Add changes from a specific file/folder to your commit. To add multiple files at once there are several options:

    * `-A` to add all new, modified, and deleted files

    * `-u` to add all modified and deleted files but not new files 

`git commit -m "commit message"`

- Commit a set of changes, make sure you have a descriptive message

`git push`

- Push your commit to the GitHub repo

- If you made a new branch locally, to add it to the origin, run `git push --set-upstream origin branch-name`

`git stash`

- This will stash your changes on your branch so you can checkout a different branch without losing your progress

- Use `git stash pop` to unstash your changes

`git pull`

- Pull the contents of the GitHub repo to your local computer 

## Advanced

If your branch is out of date from the main branch:

```
git checkout main
git pull main
git checkout branch_to_update
git rebase main
```

- If you have already made changes on your branch but want to update your branch with main, use `git stash` before you checkout main and `git stash pop` after you rebase to main. 

If a branch name changed on GitHub but it still has the old name locally and you can no longer push to/pull from the old branch name:

```
git branch -m old_name new_name
git fetch origin
git branch -u origin/new_name new_name
git remote set-head origin -a
```

If you only want to clone a specific subdirectory from a repository:

```
mkdir <repo>
cd <repo>
git init
git remote add -f origin <repo-url>
git config core.sparseCheckout true
echo "some/dir/" >> .git/info/sparse-checkout
echo "another/dir/" >> .git/info/sparse-checkout
cat .git/info/sparse-checkout # Check which folders you've added
git pull origin master
```

If you have a version of Git above 2.25.0, there is a `git sparse-checkout` command. These replace the git config and echo commands from above. 

```
git sparse-checkout init
# Same as git config core.sparseCheckout true
git sparse-checkout set "some/dir"
# Same as echo "some/dir/" >> .git/info/sparse-checkout
git sparse-checkout list
# Same as cat .git/info/sparse-checkout
```


For questions, suggestions, or to note any errors, post an issue on our [Github](https://github.com/DCAN-Labs/cdni-brain/issues).