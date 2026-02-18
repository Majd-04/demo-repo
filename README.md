[git cheat sheet](https://git-scm.com/cheat-sheet)
[git basics video](https://youtu.be/RGOj5yH7evk?si=BwCbqAFFBobD_sxY)

# Git Commands and Best Practices for Managing Repositories
## Basic Git Commands:
- `git clone <repository_url>` => Clones a repository into a new directory.
- `git status` => Shows the working tree status.
- `git add .` => Adds all changes in the current directory to the staging area.
- `git commit -m "commit message" -m "detailed description"` => Commits the staged changes with a message and a detailed description.
- `git commit -am "commit message"` => Stages and commits all changes in one command, but only for tracked files.
- `git reset` => undoes the staging of files, moving them back to the working directory without changing the commit history.
- `git reset <file name>` => unstages a specific file, moving it back to the working directory without changing the commit history.
- `git reset HEAD~1` => undoes the last commit, moving the changes back to the staging area.
- `git log` => Shows the commit history.
- `git reset <commit_hash>` => Resets the current branch to the specified commit, discarding all commits after it. Use with caution as it can lead to loss of work if not used properly.
---
- **Important Note:** If you accidentally commit sensitive files (like private keys), GitHub's Push Protection will block the push to prevent a security breach. To resolve this, you need to:
  1. Undo the commits that included the sensitive files using `git reset --soft HEAD~n` (where n is the number of commits to undo).
  2. Remove the sensitive files from the staging area with `git rm --cached <file>`.
  3. Add a proper `.gitignore` file to prevent future accidental commits of sensitive files.
  4. Re-commit and push only the necessary project files.
* **Key Lesson:** Use a `.gitignore` *before* running `git add .` to ensure secrets never enter the version control history. Once a secret is committed, you must rewrite history to remove it.
---
## Setting Up a New Git Repository on Local Machine and Connecting to GitHub:
- `git init` => Initializes a new Git repository in the current directory.
- `git add .` => Stages all changes in the current directory for the initial commit.
- `git commit -m "first commit"` => Commits the staged changes with a message
- `git branch -M main` => Renames the default branch to "main".
- `git remote add origin <repository_url>` => Adds a new remote repository with the specified URL.
- `git push -u origin main` => Pushes the committed changes to the remote repository and sets the upstream branch for future pushes. so that you can simply use `git push` in the future without specifying the remote and branch.
- `git remote -v` => Verifies the remote repository configuration by listing the remote URLs.
---
## GitHub SSH Key Management:
- `ssh-keygen -t rsa -b 4096 -C "email@example.com"` => Generates a new SSH key pair for - authentication.
- `ls | grep testkey` => Lists the generated SSH key files, which typically include:
      - testkey => The private key that should be kept secure and not shared.
      - testkey.pub => The public key that can be added to GitHub for authentication.
- `cat testkey` => Displays the contents of the private key, which should not be shared or exposed.
- `cat testkey.pub` => Displays the contents of the public key, which can be copied and added to GitHub.
- `pbcopy < testkey.pub` => Copies the contents of the public key to the clipboard for easy pasting into GitHub.
- on GitHub, navigate to Settings > SSH and GPG keys > New SSH key, then paste the public key and save it.
- `eval "$(ssh-agent -s)"` => Starts the SSH agent in the background.
- `ssh-add testkey` => Adds the private key to the SSH agent for authentication.
---
## Pushing Changes to GitHub:
- `git push origin main` => Pushes the committed changes to the remote repository on GitHub.
- `git push` => Pushes the committed changes to the remote repository. If the upstream branch is set, it will push to that branch; otherwise, it may require specifying the remote and branch.
---
## Branching and Merging:
- `git branch` => Lists all local branches in the repository. The current branch will be highlighted with an asterisk (*).
- `git checkout -b <branch_name>` => Creates a new branch with the specified name and switches to it.
- `git checkout <branch_name>` => Switches to the specified existing branch.
- `git diff <branch_name>` => Shows the differences between the current branch and the specified branch.
- to merge changes from a feature branch to the main branch, you would typically:
go to github and create a pull request from the feature branch to the main branch, review the changes, and then merge the pull request. This process allows for code review and ensures that only approved changes are merged into the main branch.
- `git pull` => Fetches and integrates changes from the remote repository into the current branch. This is useful for keeping your local branch up to date with the latest changes from the remote repository.
- `git branch -d <branch_name>` => Deletes the specified local branch. Use `-D` to force delete if the branch has unmerged changes.
- `git merge <branch_name>` => Merges the specified branch into the current branch. Mergin on local machine is not recommended for collaborative projects; instead, use pull requests on GitHub to manage merges and code reviews. But while working on a local feature branch, the main branch may receive updates from other collaborators. To keep your feature branch up to date, you should merge the latest changes from the main branch into your feature branch. This can be done with `git merge main` while you are on your feature branch. This ensures that your branch has the latest changes and helps to avoid conflicts when you eventually create a pull request to merge back into main.
---
## Forking and Pull Requests:
- To contribute to a project you don't have write access to, you can fork the repository on GitHub, which creates a copy of the repository under your account. You can then clone your fork to your local machine, make changes, and push them back to your forked repository. After that, you can create a pull request from your forked repository to the original repository, proposing your changes for review and potential merging by the maintainers of the original repository.
---
## Best Practices for Managing Git Repositories:
- Always write clear and descriptive commit messages to explain the purpose of each commit.
- Use branches to work on new features or bug fixes without affecting the main codebase.
- Regularly pull changes from the remote repository to keep your local branch up to date.
- Use pull requests for code reviews and to facilitate collaboration with other developers.
- Avoid committing sensitive information, such as API keys or passwords, to the repository. Use environment variables or a secure vault to manage sensitive data.
- Regularly clean up old branches that are no longer needed to keep the repository organized.
- Use `.gitignore` to exclude files and directories that should not be tracked by Git, such as build artifacts, log files, and sensitive information.
- Always test your changes locally before pushing them to the remote repository to ensure that they work as expected and do not introduce bugs.
- Communicate with your team members about the changes you are making and coordinate on larger features or refactoring efforts to avoid conflicts and ensure a smooth development process.

