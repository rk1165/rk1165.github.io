+++
title = 'Git Notes'
date = 2024-02-11

+++

### log

- `git log --pretty=oneline` displays commit and comment
- `git log --pretty=oneline --max-count=2`
- `git log --pretty=oneline --since='5 minutes ago'`
- `git log --pretty=oneline --until='5 minutes ago'`
- `git log --pretty=oneline --author=<your name>`
- `git log --pretty=oneline --all`
- `git log --oneline --abbrev-commit -decorate`. The `-decorate` is the key for showing HEAD and the branch tips
- `git log --all --pretty=format:'%h %cd %s (%an)' --since='7 days ago'` to review changes made in the last week by me.
- `git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short`
  - `%h` is the abbreviated hash of the commit
  - `%d` are any decorations on that commit (e.g. branch heads or tags)
  - `%ad` is the author date
  - `%s` is the comment
  - `%an` is the author name
  - `--graph` informs git to display the commit tree in an ASCII graph layout
  - `--date=short` keeps the date format nice and short
- `git log --stat` can be used to display the files that have been changed in the commit, as well as the number of lines that have been added or deleted
- `git log --patch` or `git log -p` can be used to display the actual changes made to a file.
- `git log -p -w` will show the patch information, but will not highlight lines where only whitespace changes have occurred.
- `git log --grep=bug` or `git log --grep bug` to filter commits
- `git log --name-status` : shows whether a file was Added, Modified or Deleted.
- `git log -p hello.txt` : show changes in the file named hello.txt
- `git log $file_name` : look at the history for file with given file name
- `git log --oneline $directory` : shows the commit made to the directory
- `git log tag1..tag2 --oneline | wc -l` how many commits were made between tag1 and tag2
- `git log --oneline --author $author_name` : show commits on a line by given author
- `git log -S $search_term` : search for the given term in logs
- `git log main..my-branch` : show me all the commits that are in main but not in `my-branch`. Subtract the first one from the second one
- `git log --oneline --right-only master...hotfix-1` : find commits that are in one branch but not another.
- `git log -G <regexp>` Search for commits whose changes include your regexp
- `git log --grep=login --author=travis --since=1.month` Find commits whose message mention login and were authored by Travis in the last month

### tag

- There are two types of tags in Git - **lightweight tags** and **annotated tags**. Both of them are references stored in `.git/refs/tags`.
- `git tag -a -m "Tagged 1.0" 1.0` creates annotated tag
- Annotated tags are **tag objects**, separate to the commit that it points to. Tag objects can be signed with a GPG key.
- Tag objects also store a tag message and information about the tagger.
- `git tag -a v1.0` the -a flag is used. This flag tells Git to create an annotated flag.
- Annotated tags are recommended because they include a lot of extra information such as:
  - the person who made the tag
  - the date the tag was made
  - a message for the tag.
- `git tag v1` Now I can refer to the current version of the program as v1.
- `git tag` to view tags
- `git tag -d oops` removes the tag and allow the commits it referenced to be garbage collected.
- `git tag -d v1.0` to delete a tag.
- `git tag -a v1.0 a87984` to add tag to a specific commit

### hist

- `git hist master --all` check for tags in the log
- `git hist --hard <tag>` resets the branch to the tagged commit. The `--hard` parameter indicates that the working directory should be updated to be consistent with the new branch head.

### diff

- `git diff`                         : changed and not staged for commit
- `git diff --cached`                : to see the changes you just staged.
- `git diff HEAD`                    : changed since last commit
- `git diff <commit>`                : shows changes from the given commit till latest one
- `git diff --cached <commit>`       : Specific commit and staged
- `git diff <commit> <commit>`       : difference between two commits
- `git diff feature master`          : difference between tips of branches
- `git diff feature...master`        : changes in master since feature was started off of it
- `git diff feature master file.txt` : difference in file.txt on two branches
- `git diff -w`                      : ignore whitespace related changes
- `git diff commit_id file_name`     : shows different between the file at given commit id to the one at master branch
- `git diff tag1 tag2 --stat`        : how many files have changed between tag1 and tag2
- `git diff commit_id1..commit_id2 $file_name` : gets a diff for given file name between the commits
- `git diff --staged --no-renames`
- `git diff --color-words`

### remote

- `git remote show origin`
- `git remote add shared ../hello.git` Adds the bare repository to our original repository (when this is done one can see in his local repository what has been done).
- `git remote rm <name>` to remove the remote `<name>`
- `git remote add [remote-repo-name] [remote-repo-URL]`: Records a new location for network data transfers.
- `git remote -v`: Lists all locations for network data transfers.

### branch

- `git branch --track greet origin/greet` : adds a local branch that tracks a remote branch.
- `git switch -c <new-branchname> [start-point]`
- `git switch branchname`
- `git branch -f master HEAD~3`           : moves (by force) the master branch to three parents behind HEAD
- `git branch -d`                         : delete only if it has already been fully merged in its upstream branch.
- `git branch -D`                         : deletes the branch "irrespective of its merged status"
- `git branch -m <oldname> <newname>`     : rename a branch while pointed to any branch
- `git branch -m <newname>`               : rename the current branch

### show

- `git show`                      : command lets us peer right into the beating heart of a commit.
- `git show commit_id:`           : looks at the diff of that commit
- `git show commit_id:filename`
- `git show tag`                  : shows the commit of the given tag
- `git show HEAD`                 : shows what commit id HEAD corresponds to now
- `git show HEAD~2^2`             : two commit back head and second parent
- `git show HEAD@{"1 month ago"}` : show where head was a month ago

### stash

- Git creates a special reference for the stash which lives in `.git/refs/stash`. We can verify this with `git show-ref`
- `git stash list`              : To list the stashed modifications
- `git stash show`              : to show files changed in the last stash
- `git stash show -p`           : to view the content of the most recent stash
- `git stash show -p stash@{1}` : to view the content of an arbitrary stash.
- To delete a normal stash created with git stash, you want `git stash drop` or `git stash drop stash@{n}`

### status

- `git status -s` : shows the files along with their status i.e modified/append/untracked

### blame

- `git blame file`
- `^` means the lines which are present since the file was first added

### Rebasing

- `git rebase <base> <target>`, the `rebase` command will take all of the commits from `<target>` and play them on top of `<base>` one by one. It does this without actually modifying `<base>`, so the end result is a linear history in which `<base>` can be fast-forwarded to `<target>`
- **Moving** a set of commits (like a branch) from one **base** to another by replaying the changes that made those commits starting from the new base.
- `git rebase` command will move commits to have a new base. In the command `git rebase -i HEAD~3`, we're telling Git to use `HEAD~3` as the base where all of the other commits (`HEAD~2`, `HEAD~1`, and `HEAD`) will connect to. The `-i` in the command stands for "interactive
- Rebase Commands
  - use `p` or `pick` – to keep the commit as is
  - use `r` or `reword` – to keep the commit's content but alter the commit message
  - use `e` or `edit` – to keep the commit's content but stop before committing so that you can:
    - add new content or files
    - remove content or files
    - alter the content that was going to be committed
  - use `s` or `squash` – to combine this commit's changes into the previous commit (the commit above it in the list)
  - use `f` or `fixup` – to combine this commit's change into the previous one but drop the commit message
  - use `x` or `exec` – to run a shell command
  - use `d` or `drop` – to delete the commit
- `git rebase --continue`

### grep

- `git grep -e <regexp> my_other_branch -- '*.js'` Search for our regexp in JavaScript files from another branch : `--` signals the end of options and the rest of the parameters are limiters.
- `git grep --cached -e <regexp>` Search files registered in the index, rather than the working tree
- `git diff-index --cached -G<regexp> HEAD | cut -f2` Search for staged files containing a regexp that was either added or removed
- `git grep -e 'include' --or 'extend' --and \( -e 'Specification' -e 'Factory' \)` Combine regexps and filter results via boolean logic : With this command, we search for lines where we extend _or_ include, either specifications _or_ factories
- `git grep --all-matches -e <regexp> -e <regexp>` Find files that contain some terms, not necessarily on the same line

### submodule

- A construct within Git that enables you to keep a separate Git repository as a subdirectory within an existing repository. This effectively keeps two separate repositories linked together within a project.
- `git submodule add repo external/repo1` : adding a submodule. Connects the submodule's git repo to our current repo.
- `git submodule init` initialize submodules
- `git submodule update` grab the content of the submodules
- `git submodule deinit external/repo1` temporarily remove the repo. For permanent removal run `git rm external/repo1` also.

### Theory

- When given a commit reference (i.e. a hash, branch or tag name), the reset command will
  - Rewrite the current branch to point to the specified commit
  - Optionally reset the staging area to match the specified commit
  - Optionally reset the working directory to match the specified commit
- `git mv file dir` Using this command informs git of 2 things
  - The file has been deleted
  - The dir/file has been created.
- in .git/objects contains directories with 2 letter names. The directory names are the first two letters of the sha1 hash of the object stored in git.
- We can reset a branch to any commit we want. Essentially this is modifying the branch pointer to point to anywhere in the commit tree.
- `git fetch` command will fetch new commits from the remote repository, but it will not merge these commits into the local branches.
- The branches starting with remotes/origin are branches from the original repo.
- Bare repositories (without working directories) are usually used for sharing.
- `git clone --bare hello hello.git` The convention is that repositories ending in ‘.git’ are bare repositories. it is nothing but the .git directory of a non-bare repo.
- `git push shared master` shared is the name of the repository receiving the changes we are pushing.
- In this case we want the diff of our most recent commit, which we can refer to using the HEAD pointer `git diff HEAD`.
- `git add [forgotten-file]`
- `git add --patch file` : to commit parts of file in hunksf
- `git checkout commitID fileName` for not being in detached HEAD state
- `git pull [remote-repo-name] master`: Get the most recent copy of the files as seen in remote-repo-name
- `git push [remote-repo-name] master`: Pushes the most recent copy of your files to the remote-repo-name.
- The body of your message should provide detailed answers to the following questions: What was the motivation for the change? How does it differ from the previous version?
- When you're ready to restore a saved Stash, you have two options:
  1. Calling `git stash pop` will apply the newest Stash and clear it from your Stash clipboard.
  2. Calling "`git stash apply <stashname>`" will also apply the specified Stash, but it will remain saved. You can delete it later via `git stash drop <stashname>`
- we also delete the remote branch by using the "git push" command with the "--delete" flag:`
- **config file** - where all project specific configuration settings are stored. Git looks for configuration values in the configuration file in the Git directory (.git/config) of whatever repository you’re currently using. These values are specific to that single repository.
- `git config --list --show-origin` : show all config values and the file where they are defined
- `git config user.name` : show the current user.name config values (sub any key)
- `git config --global --unset user.name` : remove a specific setting for a specific level of config
- `git config --global --edit` : edit a specific level of config directly
- `git config --global --remove-section user` : remove a section of config for a specific level
- **description file** - this file is only used by the GitWeb program, so we can ignore it
- **hooks directory** - this is where we could place client-side or server-side scripts that we can use to hook into Git's different lifecycle events
- **info directory** - contains the global excludes file
- **objects directory** - this directory will store all of the commits we make
- **refs directory** - this directory holds pointers to commits (basically the "branches" and "tags")
- `git revert <SHA-of-commit-to-revert>` to revert a specific commit, resetting, on the other hand, erases commits!
- Git does keep track of everything for about 30 days before it completely erases anything. To access this content use the `git reflog` command
- The main difference between the ^ and the ~ is when a commit is created from a merge. A merge commit has two parents. With a merge commit, the ^ reference is used to indicate the first parent of the commit while ^2 indicates the second parent. The first parent is the branch you were on when you ran git merge while the second parent is the branch that was merged in.
- The git reset command is used to reset (erase) commits: `git reset <reference-to-commit>`. It can be used to:
  - move the HEAD and current branch pointer to the referenced commit
  - erase commits with the `--hard` flag
  - move committed changes to the staging index with `--soft` flag
  - unstage committed changes with `--mixed` flag
- `git shortlog` displays an alphabetical list of names and the commit messages that go along with them.
- `-s` : to see the number of commits and `-n` to sort them numerically
- The first thing you should always look for in a project is a file with the name CONTRIBUTING.md.
- GitHub's issue tracker is quite sophisticated. Each issue can:
  - have a label or multiple labels applied to it
  - can be assigned to an individual
  - can be assigned a milestone (for example the issue will be resolved by the next major release)
- Another thing that's nice about issues is:
  - they let you subscribe to an issue so you'll be notified of new comments and code changes
  - you can communicate back and forth with a project maintainer on a specific change
- The best way to organize the set of commits/changes you want to contribute back to the project is to put them all on a topic branch. Now what do I mean by a topic branch? Unlike the master branch which is the default branch that holds all of the commits for your entire project, a topic branch host commits for just a single concept or single area of change
- squashing is taking a number of commits and combining it together.
- Normally HEAD points to a branch name. Detaching HEAD just means attaching it to a commit instead of a branch.
- Two simple relative commits movement:
  - Moving upwards one commit at a time with `^`
  - Moving upwards a number of times with `~<num>`
- Each time we append `^` to a ref name, we are telling Git to find the parent of the specified commit.
- Resetting works great for local branches. It doesn't work for remote branches that others are using.
- In order to reverse changes and _share_ those reversed changes we need to use `git revert`.
- `git cherry-pick <Commit1> <Commit2> <...>` : it's a very straightforward way of saying that you would like to copy a series of commits below `HEAD`.
- We can give commit ids from multiple branches also and it works fine.
- When the interactive rebase dialog opens, we have the ability to do three things:
  1. Reorder commits by changing their order in the UI.
  2. Choose to completely omit some commits. This is designated by `pick` -- toggling `pick` off means we want to drop the commit.
  3. We can squash commits. It allows to combine commits.
- `.gitattributes` Specific configuration settings placed on a file or collection of files within a repository that dictate how the file is handled. In some cases, different Git hosts may offer specific functionality that leverages these attributes.
- Git attributes common uses
  - Configuring line ending in files
  - Specifying binary files
  - Enabling large binary files to be handled by Git LFS
  - Excluding files from exported version of repository
  - Specifying clean and smudge filters : to hide security things
- We can use exif tool to compare images.

### Git Under the Hood

- At its core git is a persistent Map.
- Values are any sequence of bytes. The content of a text file. Git generates the hash using SHA1 algorithm.
- `echo "Apple Pie" | git hash-object --stdin`
- `echo "Apple Pie" | git hash-object --stdin -w` : saves it in .git/objects/commit_id
- `git cat-file id -t` : tells us what this piece of content is
- `git cat-file id -p` : pretty prints the object
- `git count-objects` : to count the objects
- `git show-ref main`
- A git repository is just a simple key-value data store. This is where Git stores, among other things:
  - **Blobs** : a blob is just a bunch of bytes; usually a binary representation of a file.
  - **Tree objects**, which are bit like directories. They can contain pointers to blobs and other tree objects.
  - **Commit objects**, which point to a single tree object, and contain some metadata including the commit author and any parent commits.
  - **Tag objects**, which point to a single commit object, and contain some metadata.
  - **References**, which are pointers to a single object.
- The commit itself has a hash, which is built from a combination of the metadata that it contains:
  - The hash of the tree at the time of the commit.
  - The hash of any parent commits.
  - The author's name and email address and the time that the changes were authored.
  - The committer's name and email address, and the time that the commit was made.
  - The commit message
- A reference is simply a file stored somewhere in `.git/refs` containing the hash of a commit object.
- Git branches are just references.
- When Git mentions **fast-forward** during the merge, what this means is that all of the commits in `hotfix` were directly upstream from `master`. This allows Git to simply move the `master` pointer up the tree to `hotfix`
- It is possible to have a repository which can store a project's history without actually having a working tree. This is called a _bare_ repository.
- With the format `git pull <remote> <branch>`, the `git pull` command does the following:
  1. Run `git fetch <remote>`
  2. Reads `.git/FETCH_HEAD` to figure out if `<branch>` has a remote-tracking branch which should be merged
  3. Runs `git merge` if required
- When we make a change in Git that affects the tip of a branch, Git records information about that change in what's called the reflog.
- The reflog shows a list of all changes to HEAD in reverse chronological order. `git reflog`
- `git fsck` is able to check Git's database and verify the validity and reachability of every object that it finds.
- `git describe` show the most recent tag that is reachable from a commit.
- `git-rev-parse` the most common use is figuring out which commit a tag or branch points to.
- `git bisect` is a tool when you need to figure out which commit introduced a breaking change.
- `git branch --contains HEAD`: find which branches a commit is in
- `git cat-file -p HEAD` : view details of an object.
- `git cat-file -t HEAD` : show an object's type.
- `git cat-file -s HEAD` : show an object's size
- `git ls-tree -t -r HEAD` : print the tree of a given reference
- `git describe HEAD` : find the first tag that contains a reference

### Some Links

- whether you should merge or rebase is a hot topic! some good reads:
  - [this](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
  - [this](https://derekgourlay.com/blog/git-when-to-merge-vs-when-to-rebase/)
  - [this](https://stackoverflow.com/questions/804115/when-do-you-use-git-rebase-instead-of-git-merge)
- [git extras](https://github.com/tj/git-extras/blob/master/Commands.md) provides a bunch of little utilities that integrate with git
- Git has [tig](https://github.com/jonas/tig), try installing it and running it in a repo. You can find some usage examples [here](https://www.atlassian.com/blog/git/git-tig).

### Git Questions

- How to merge another developer's branch into mine?
  - `git remote add otherRepo uriToOtherRepo`
  - `git fetch otherRepo`
  - `git merge otherrepo/branch $your_branch_name`
- `git merge --abort` : to abort a merge
- How do I check out a remote git branch?
  - `git checkout -b <local_branch_name> <name_of_remote>/<remote_branch_name>`
  - `git checkout -t <name_of_temo>/<remote_branch_name>`
- How do I execute a Git command without being in the repository?
  - `git --git-dir=/home/repo/.git log`
  - `git -C /home/repo log`
- How to I delete a branch locally and remotely?
  - `git push <remote_name> -d <branch_name>`
  - `git branch -d <branch_name>`
- Read Pro Git : 10, 2, 3, 7
- `git log -n100000 --format="%ae" | cut -d@ -f2 | sort | uniq -c | sort -nr | less` : gives the email of the companies who contributed to a repo
- `git update-index --assume-unchanged <file>` : to gitignore a particular file/dir
- `git update-index --no-assume-unchanged <file>` : to undo the above steps
- `git ls-files -v | grep '^h'` : to get a list of dir's/files that `assume-unchanged`
