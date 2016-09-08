# Git Cheat Sheet

This is a fork of [Alex Kras Gist](https://gist.github.com/akras14/3d242d80af8388ebca60). I've made this fork as a base to add my own notes. Home of this fork is [joede-git-cheat-sheet.md](https://gist.github.com/joede/89b3fb9f113fbfafd486acd3be077070).

## Commands

### Getting Started

- `git init` to start a new repository
- `git clone url` to clone an exisiting remote repository

### Configuration

```
git config --global color.ui true
git config --global push.default current
git config --global core.editor vim
git config --global user.name "John Doe"
git config --global user.email foo@citrix.com
git config --global diff.tool meld
```

### Working with Local Branch

#### Branching

See the list of all local branches.
```bash
git branch
```

Switch to existing local branch.
```bash
git checkout branchname
```

Checkout current branch into a new branch, named new-branch-name.
```bash
git checkout -b new-branch-name
```

Merge branch-name into the current branch.
```bash
git merge branchname
```

Merge branch without fast forwarding. This is what pull requests do.
It helps to preserve history of the changes as relavant to that branch
It's an advanced feature, but try it out with GUI to see the difference
between the regular merge and merge `--no-ff`.
```bash
git merge --no--ff branchname
```

Soft branch delete, will complain if the branch is not merged.
```bash
git branch -d branchname
```

Hard branch delete, will not complain about nothing. Like `rm -rf` in bash.
```bash
git branch -D branchname
```

#### Updating Current Branch -- Standard Flow

See all commits
```bash
git log
```

Pretty commit view, you can customize it as much as you want.
Just google it :)
```bash
git log --pretty=format:"%h %s" --graph
```

See what you worked on in the past week.
```bash
git log --author='Alex' --after={1.week.ago} --pretty=oneline --abbrev-commit
```

See only changes made on this branch (assuming it was branched form master branch).
```bash
git log --no-merges master..
```

See status of your current git branch. Often will have advice on command that you need to run.
```bash
git status
```

Short view of status. Helpful for seeing things at a glance.
```bash
git status -s
```

Add modified file to be commited (aka stage the file).
```bash
git add filename
```

Add all modified files to be commited(aka stage all files)
```bash
git add .
```

Add only text files, etc.
```bash
git add '*.txt'
```

Tell git not to track file anymore.
```bash
git rm filename
```

Record changes to git. Default editor will open for a commit message. (Visible via git log)
Once files are commited, they are history.
```bash
git commit
```

 A short hand for commiting files and writing a commit message via one command.
```bash
git commit -m 'Some commit message'
```

Changing the history :) If you want to change your previous commit,
you can, if you *have not pushed* it yet to a remote repository.
Simply make new changes, add them via git add, and run the following command.
Past commit will be ammended.
```bash
git commit --amend
```

#### Updating Current Branch -- Advanced Flow

Unstage pending changes, the changes will still remain on file system.
```bash
git reset
```

Unstage pending changes, and reset files to pre-commit state.
```bash
git reset --hard HEAD
```

Go back to some time in history, on the current branch.
```bash
git reset tag
git reset <commit-hash>
```

Save current changes, without having to commit them to repo...
```bash
git stash
```

...And later return those changes.
```bash
git stash pop
```

Return file to it's previous version, if it hasn’t been staged yet.
Otherwise use `git reset filename` or `git reset --hard filename`.
```bash
git checkout filename
```

#### Updating Current Branch -- Comparing changes

See current changes, that have not been staged yet.
Good thing to check before running git add
```bash
git diff
```

See current changes, that have not been commited yet (including staged changes).
``` bash
git diff HEAD
```

Compare current branch to some other branch.
``` bash
git diff branch-name
```

Same as diff, but opens changes via difftool that you have configured.
The option `-d` tells it to open it in a directory mode, instead of having to open
each file one at a time.
``` bash
git difftool -d
```

See only changes made in the current branch (compared to master branch).
Helpful when working on a stand alone branch for a while.
``` bash
git difftool -d master..
```

See only the file names that has changed in current branch.
```bash
git diff --no-commit-id --name-only --no-merges origin/master...
```

Similar to above, but see statistics on what files have changed and how.
```bash
git diff --stat #Your diff condition
```

### Working with Remote Branch

See list of remote repos available. If you did git clone,
you'll have at least one named "origin".
```bash
git remote
```

Detailed view of remote repos, with their git urls.
```bash
git remote -v
```

Add a new remote. I.e. origin if it is not set.
```bash
git remote add origin <https://some-git-remote-url>
```

Push current branch to remote branch (usually with the same name)
called upstream branch.
```bash
git push
```

If a remote branch is not set up as an upstream, you can make it so.
The `-u` tells Git to remember the parameters.
```bash
git push -u origin master
```

Otherwise you can manually specify remote and branch to use every time.
```bash
git push origin branchname
```

Just like pushing, you can get the latest updates from remote.
By defaul Git will try to pull from "origin" and upstream branch.
```bash
git pull
```

Or you can tell git to pull a specific branch.
```bash
git pull origin branchname
```

Git pull, is actually a short hand for two command.
Telling git to first fetch changes from a remote branch.
And then to merge them into current branch.
```bash
git fetch && git merge origin/remote-branch-name
```

If you want to update history of remote branches, you can fetch and purge.
```bash
git fetch -p
```

To see the list of remote branches `-a` stands for all.
```bash
git branch -a
```

## Resources
### Reference
- Try [Github](https://try.github.io)
- [lernGitBranching](http://pcottle.github.io/learnGitBranching/?NODEMO)
- Free Book: [Pro Git](http://git-scm.com/book)

### Git Frontends and Tools
- `tig` is a ncurses-based text-mode interface for Git (install: `sudo apt-get install tig`)
- `gitg` is a fast, GTK based Git frontend to browse, repositories, commit changes, handle branches (install: `sudo apt-get install gitg`)
- [Source Tree](http://www.sourcetreeapp.com/) -- Atlassians free Git and Mercurial client for Windows or Mac. Note, SourceTree is not open source!

### Merge/Diff Tools
- Meld is a graphical diff viewer and merge application for the GNOME desktop. It supports Git, Mercurial, SVN, CVS, ... (install: `sudo apt-get install meld`)
- [p4v Merge](http://git-scm.com/book/en/Customizing-Git-Git-Configuration#External-Merge-and-Diff-Tools)
