# Most common git screwups/questions and solutions

[http://41j.com/blog/2015/02/common-git-screwupsquestions-solutions/](http://41j.com/blog/2015/02/common-git-screwupsquestions-solutions/)

I was looking to learn a bit more about the parts of git I’ve not
ventured into yet. What better way that looking the most common ways
people screw them up and how to fix the resulting problems! Here’s a
short list, compiled from my own experience and issues I’ve come across
on the Internet.

## I wrote the wrong thing in a commit message

If the commit hasn’t been push you can do the following, which will
allow you to edit the message on the most recent commit:

~~~~
git commit --amend
~~~~

## How can I undo the last commit?

You can use git reset e.g.:

~~~~
git reset --hard HEAD~1
~~~~

`HEAD\~1` means HEAD-1 commit. It should be noted that this is the nuclear
option, and any changes you made will be **discarded**. If you want to keep
your changes in the working tree use:

~~~~
git reset --soft HEAD~1
~~~~

If you’ve already published your commits, you should use revert. This is
create new commits undoing the last change:

~~~~
git revert HEAD~1..HEAD
git revert commitid
~~~~

**Note:** the revert command is a safer alternative to the reset
command, since the commit objects are still available!

## Delete a Git branch remotely

Git knows two ways to delete a branch inside the remote repository only.
The shorter version is available since git v1.5.0.

~~~~
git push origin --delete branchname
git push origin :branchname
~~~~

## What are the differences between `git pull` and `git fetch`?

git pull, is git fetch followed by git merge. git fetch gets the remote
changes, they get kept under `refs/remotes/`

## How do I undo a `git add` before committing

You did a `git add filename` accidentally and want to undo it before
committing your change. You can simply do:

~~~~
git reset filename
~~~~

To unstage your changes to that file.

## How do I deal with merge conflicts

Use `git mergetool` which gives a handy interface for solving merge
conflicts.

## Remove all local untracked files (and directories) from your local clone

Careful! You might want to take a backup before doing this:

~~~~
git clean -f -d
~~~~

## Clone all remote branches

You probably already have, they’re just hiding! Use the following to see
all the branches:

~~~~
git branch -a
~~~~

You can then use `git checkout origin/branchname` to take a look at the
branch. Or `git checkout -b branchname origin/branchname`. To create a
local tracking branch.

## Rename local branch?

~~~~
git branch -m oldname newname
~~~~

## Revert to a previous Git commit

You can use reset as above to revert to a previous commit, this assumes
you mean go back to where you were before permanently rather than just
take a look (for that you want to checkout an old version). The commit
ID, should be as shown in the output of `git log`.

~~~~
git reset --hard commitid
~~~~

Again this will discard all changes in your working tree, so make sure
this is really what you want to do! Or look at using --soft rather than
--hard.

## Remove a git submodule

Creating a submodule is pretty straight-forward, but deleting them less
so the commands you need are:

~~~~
git submodule deinit submodulename
git rm submodulename
git rm --cached submodulename
rm -rf .git/modules/submodulename
~~~~

## Over-write local files when doing a git pull

Git reset is your friend again:

~~~~
git fetch --all
git reset --hard origin/master
~~~~

## How can I add an empty directory to my repository?

You can’t! git doesn’t support this, but there’s a hack. You can create
a .gitignore file in the directory with the following contents:

~~~~
# Ignore everything in this directory
*
# Except this file
!.gitignore
~~~~

I don’t believe it actually matters what’s in the .gitignore (and this
.gitignore will ignore all files).

## git export, exporting the source code as with `svn export`

Use git archive e.g:

~~~~
git archive --format zip --output /full/path/to/zipfile.zip master
~~~~

## Discard all my unchecked in changes

~~~~
git checkout -- .
~~~~

## Create a new remote branch from the current local one

~~~~
git config --global push.default current
git push -u
~~~~

## Restore a deleted file

First find the commit when the file last existed:

~~~~
git rev-list -n 1 HEAD -- filename
~~~~

Then checkout that file

~~~~
git checkout deletingcommitid^ -- filename
~~~~

## Revert a single file to a specific previous commit

Similar to the above, but a bit simpler:

~~~~
git checkout commitid filename
~~~~

## Make git ignore permissions/filemode changes

~~~~
git config core.fileMode false
~~~~

## Get git to remember my password when checking out with https

It’ll only remember your password for 15mins:

~~~~
git config --global credential.helper cache
~~~~

You can make it remember your password longer with:

~~~~
git config --global credential.helper "cache --timeout=XXXX"
~~~~

## Take a quick look at an old revision of a single file

~~~~
git show commitid:filename
~~~~

# Tips find in the comments


## you want to undo the last rebase you completed

~~~~
git reset --soft ORIG_HEAD
~~~~

(or --hard if you’re confident/adventurous)

## I lost my commit somehow (or made a bad amend)

~~~~
git reflog
~~~~

then use one of the commands above that take a commitid.