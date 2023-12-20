# Other GitHub Tips

## Good Practices

1. Make your commit messages short but descriptive.
    - "changed stuff" -> "added comments to run.py funcs"
2. Commit often
    - This ensures that you don't lose information.
    - Also, it makes it so you have less very large commit messages.
2. Combine your work and personal GitHub's sooner rather than later.
    - Having these separate instinctively seems correct, but it is easier to have one GitHub account to maintain.
3. Pin your biggest code projects to the top of your profile.
    - In this lab, we interact with A LOT of codebases quite regularly. Pinning your main projects ensures they don't get lost.
4. There are certain commands that are easier on command line, and certain commands that are easier with IDE extensions.
    - EX: Commits, staging commits, pushes, pulls (everyday tasks) are easier via [vscode extension](vscode.md).
    - EX: [Scrubbing a repository](github-scrub.md) of a commit or data within a commit is easier via command line.
5. Put a README.md within all your codebases, even if you are the only person using it.
    - You will forget how to run you own code, trust me.
6. Get a DOI for your codebase if it is being used by others.
    - This is usually done via [zenodo](zenodo.md).
    - People will be able to credit you and reference specific commits with this.
7. [Add a README.md to your profile](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/customizing-your-profile/managing-your-profile-readme).
    - Include your linked in, projects your working on, your focus, other websites, recent publications, etc
    - More information on how to personalize your GitHub profile can be found [here](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/customizing-your-profile/personalizing-your-profile).
8. Keep your branches updated
    - Frequently [pull to and sync from your local branch](https://docs.github.com/en/desktop/working-with-your-remote-repository-on-github-or-github-enterprise/syncing-your-branch-in-github-desktop)
    - When intending to submit a [pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests), merge with the branch you're intending to submit the PR to locally before initiating the request. This ensures you won't have any merge conflicts.

## Watch a Repo 

Watching a repo will enable email notifications any time a certain event happens on the repo. This is a good way to stay in the know about what's going on in your repo. To watch a repo, follow these steps:

- On the repo you want to watch, in the upper right corner, there should be a `watch` menu that you can select
- Select what level you want to watch at
    * You can create a custom option if you only want to be notified of certain events (pull requests and issue postings for example)
