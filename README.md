- git clone <repository_url> => Clones a repository into a new directory.
- git status => Shows the working tree status.
- git add . => Adds all changes in the current directory to the staging area.
- git commit -m "commit message" -m "detailed description" => Commits the staged changes with a message and a detailed description.
- ssh-keygen -t rsa -b 4096 -C "email@example.com" => Generates a new SSH key pair for - authentication.
- ls | grep testkey => Lists the generated SSH key files, which typically include:
      - testkey => The private key that should be kept secure and not shared.
      - testkey.pub => The public key that can be added to GitHub for authentication.
- cat testkey => Displays the contents of the private key, which should not be shared or exposed.
- cat testkey.pub => Displays the contents of the public key, which can be copied and added to GitHub.
- pbcopy < testkey.pub => Copies the contents of the public key to the clipboard for easy pasting into GitHub.
- on GitHub, navigate to Settings > SSH and GPG keys > New SSH key, then paste the public key and save it.
- eval "$(ssh-agent -s)" => Starts the SSH agent in the background.
- ssh-add testkey => Adds the private key to the SSH agent for authentication.
- git push origin main => Pushes the committed changes to the remote repository on GitHub.
- git push => Pushes the committed changes to the remote repository. If the upstream branch is set, it will push to that branch; otherwise, it may require specifying the remote and branch.
- git branch => Lists all local branches in the repository. The current branch will be highlighted with an asterisk (*).
- git checkout -b <branch_name> => Creates a new branch with the specified name and switches to it.
- git checkout <branch_name> => Switches to the specified existing branch.
- git merge <branch_name> => Merges the specified branch into the current branch.
- git diff <branch_name> => Shows the differences between the current branch and the specified branch.
- to merge changes from a feature branch to the main branch, you would typically:
go to github and create a pull request from the feature branch to the main branch, review the changes, and then merge the pull request. This process allows for code review and ensures that only approved changes are merged into the main branch.

- **Important Note:** If you accidentally commit sensitive files (like private keys), GitHub's Push Protection will block the push to prevent a security breach. To resolve this, you need to:
  1. Undo the commits that included the sensitive files using `git reset --soft HEAD~n` (where n is the number of commits to undo).
  2. Remove the sensitive files from the staging area with `git rm --cached <file>`.
  3. Add a proper `.gitignore` file to prevent future accidental commits of sensitive files.
  4. Re-commit and push only the necessary project files.
- git init => Initializes a new Git repository in the current directory.
- git remote add origin <repository_url> => Adds a new remote repository with the specified URL.
- git remote -v => Verifies the remote repository configuration by listing the remote URLs.
- git push -u origin main => Pushes the committed changes to the remote repository and sets the upstream branch for future pushes. so that you can simply use `git push` in the future without specifying the remote and branch.
---
To add this to your notes, here is a concise breakdown of the situation:

### **Issue: Git Secret Leak & Push Block**

* **The Problem:** I accidentally staged and committed sensitive files (`testkey`, `testkey.pub`) using `git add .`. GitHub’s **Push Protection** detected the private SSH key and blocked the push to prevent a security leak.
* **Why `.gitignore` alone didn't fix it:** Even after adding the keys to `.gitignore`, they remained in the **Git History** of previous commits. Git attempts to push the entire history, not just the current state of files.
* **The Solution:**
1. **Undo the "dirty" commits:** Used `git reset --soft` to roll back the history while keeping the file changes.
2. **Remove from index:** Used `git rm --cached` to stop Git from tracking the keys.
3. **Clean Commit:** Re-committed only the legitimate project files.


* **Key Lesson:** Use a `.gitignore` *before* running `git add .` to ensure secrets never enter the version control history. Once a secret is committed, you must rewrite history to remove it.

