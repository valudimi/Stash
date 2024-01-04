# Commands
```
git clone <project>
git init
git remote add origin <project>
git status                                         # Provides info about the current branch, staging area etc.
git log <--oneline>                                # CLI view of commit history
git diff <id1> <id2>                               # argc: 0 (versus HEAD), 1 (id vs HEAD) or 2 (id vs id)
git diff --cached                                  # diff between HEAD and data in staging area
git add <-i> <path>

git reset <path>
git commit <-s -a -m "MESSAGE">                    # Always sign your commits
git commit --amend <--author "<name> <email>">

git branch <branch>                                # Create a new branch
git branch -d <branch>                             # Delete a branch
git push -u origin <branch>
git pull origin <branch>                           # Update branch with changes from cloud

git checkout -b <branch>                           # Switch to specified branch with local changes
git checkout -b origin/<branch>                    # Switch to specified branch with changes from cloud

git merge <branch>                                 # Bring changes from specified to current branch
git rebase <branch>                                # Move all commits in chronological order (current to specified)
git rebase <destination_branch> <source_branch>    # As above, but source to destination
git rebase -i <id>                                 # Used to modify a specific commit

git stash                                          # Save current changes and return the project to last commit's state
git stash pop                                      # Apply stash changes again and delete them from temporary storage

git cherry-pick <commit_hash>                      # Take all changes from specific commit and apply to current branch
git rebase -i Head~<N>                             # Squash last N commits into a single one, replace 'pick' with 'squash' except first line
```



# Rules
- Commits are snapshots of the entire project, so make sure to **keep the repository in a compilable state** with each commit (you can break things, but keep compilability)
    - Use `diff --cached` to check things before committing
- Atomic commits - **each commit should aim to do a single thing**, and to do it well; each fix should go into a separate commit
- Keep commit messages short (ideally below 50 characters for the first line; this is because some tools use it as an e-mail subject line), use present tense (ensures uniformity with messages used by tools such as `git merge`), write sentences, not descriptions, and end with a dot



# Configuration and good practices
### After installation
```
git config --global user.name "<name>"
git config --global user.email "<email>"
git config --global color.ui auto
```

### Commit from different account
If you're SSH'd into a different machine or in a similar situation and you want to do multiple commits, you can use:
```
export GIT_AUTHOR_NAME="<name>"
export GIT_AUTHOR_EMAIL="<email>"
```

If you only want to do one, you can use:
```
git commit --author "<name> <email>"
```

### Split commits into atomics
If you've made many changes that need to be split into atomic commits, you can `git add -i` as follows:
1. Choose `patch` (press `p` or `5`)
2. Choose the file you want to split
3. Press `Enter`
4. Answer `y`/`n` to include/exclude chunks
5. `q` to quit
After this, the modified file will be found in the `staging`` area only with the selected chunks. In the `changes` area, it will be found with all of the previous changes.


### Change existing commits
To change existing commits, you have a few different options, but keep in mind that these are all for local ones. If you want to change the latest one, first use `git add` and then:
```
git commit --amend <--author "<name> <email>">
```

To modify a specific commit:
```
git rebase -i <id>
git add <...>
git commit --amend
git rebase --continue
```

### Ignoring
Usually, a repository should only cover the source code - text files - that are needed for compilation, not images, compressed files etc., so a basic `.gitignore` in the root folder should generally cover the following minimum:
```
*~
*.swp
*.swo
*.o
*.obj
*.a
*.so
*.dll
*.lib
*.gz
*.bz2
*.zip
```
You should only commit source code files or files that cannot be compiled/linked from other files. You can also create `.gitignore` files in subfolders as necessary. `.gitignore` files are committed and their exclusion rules are applied to all contributors. If you have a file/folder you're creating for your own use and adding it to `.gitignore` would create complications, edit `.git/info/exclude`, which is completely local to your clone and follows the same syntax as `.gitignore`.

`.gitignore` and `.git/info/exclude` only cover files that aren't tracked. If you want to ignore a file that is already tracked, you need to run `git update-index --assume-unchanged <file>`. Any local updates to this `<file>` will henceforth not be taken into account in subsequent commits.

### Cleanup
- Clear any updates to a file that's being tracked: `git checkout <file>`
- Remove file from staging area and place it in the modified state: `git reset HEAD <file>`
- Clear non-tracked files from the working clone: `git clean <file>`
- Clear all non-tracked files from working clone: `git clean -f`
- Clear all tracked file changes and revert to the initial state of `HEAD` (doesn't affect non-tracked files): `git reset --hard`

# Resources
- [Commits are snapshots, not diffs](https://github.blog/2020-12-17-commits-are-snapshots-not-diffs/)
- [gitready](https://gitready.com/)
- [git immersion](https://gitimmersion.com/)
- [tpope's post on commit messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
- [Pro Git book](https://git-scm.com/book/en/v2)
- [Developer's Certificate of Origin](https://www.kernel.org/doc/html/latest/process/submitting-patches.html#developer-s-certificate-of-origin-1-1)