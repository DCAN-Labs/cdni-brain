# GitHub History Scrub: Removing Sensitive Data

If you need to remove a file or directory containing sensitive data from your Git repository's history on GitHub, follow these steps:

## Removing a File

To remove a specific file from the Git history, use the following command:

```
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch PATH-TO-YOUR-FILE" --prune-empty --tag-name-filter cat -- --all
```

## Removing a Directory

To remove an entire directory from the Git history, use this command:

```
git filter-branch --tree-filter 'rm -rf PATH-TO-YOUR-DIRECTORY' --prune-empty
```

After you've executed either of the above commands, follow the next steps, which are the same for both file and directory removal:

## Clean Up Refs

Remove references to original commits with the following commands:

```
git for-each-ref --format="%(refname)" refs/original/ | xargs -I {} git update-ref -d {}
```

## Optimize the Repository

Run Git Garbage Collection to optimize the repository:


```
git gc --prune=now
git gc --aggressive --prune=now
```

## Force Push to GitHub

To apply the changes to your remote GitHub repository, use force pushes for both branches and tags:

```
git push origin --force --all
git push origin --force --tags
```
For more information on these commands, you can also refer to this source: [How to remove sensitive files and their commits from Git history.](https://www.educative.io/answers/how-to-remove-sensitive-files-and-their-commits-from-git-history)

By following these steps, you can effectively scrub sensitive data from your Git repository's history, making it safe for public sharing without exposing confidential information.