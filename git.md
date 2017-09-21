# Git

## Commands

### Basics

* Add file to the Index (Stage)

```
$ git add <filename>
$ git add *
```

This is the first step in the basic git workflow.

* To actually commit these changes use

```
$ git commit -m "Commit message"
```

Now the file is committed to the HEAD, but not in your remote repository yet.

* Commit after staging

```
$ git commit -a -m "Commit message"
```
> -a (--all): automatically stage files that have been modified and deleted, but new files you have not told Git about are not affected.

* Rename a file

```
$ git mv <old_filename> <new_filename>
```

* Set/Show git username

```
$ git config [--global] user.name "<username>"
```

### Branching

* Create a new branch and switch to it

```
$ git checkout -b <branch_name> [<start point>]
```

* Switch to branch

```
$ git checkout <branch_name>
```

* Delete branch

```
$ git branch -d <branch_name>
```

### Pushing changes

* Connect repository to a remote server

```
$ git remote add origin <server>
```

* Push the branch to remote repository

```
$ git push origin <branch_name>
```

* Remove remote branch

```
$ git push origin :<branch_name>
```

### Update and merge

* Update local repository to the newest commit (fetch and merge remote changes)

```
$ git pull #in your working directory
```

* Merge another branch into active branch (e.g. master)

```
$ git merge [--no-ff] [-X <strategy-option>] <branch_name>
```
> The --no-ff flag avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature.

> The -X option specifies merge strategy, including theirs and ours. Use "theirs" when trying to updata the current branch to a newer branch.

> In both cases git tries to auto-merge changes. Unfortunately, this is not always possible and results in conflicts. You are responsible to merge those conflicts manually by editing the files shown by git.


* Add manually merged files after editing

```
$ git add <filename>
```

* Preview them by using before merging changes

```
$ git diff <source_branch> <target_branch>
```

* Prefer theirs(ours) when merging to avoid conflict

```
$ git pull -X theirs(ours)
```

* Accept theirs(ours) if already in conflicted state

```
$ git checkout --theirs(ours) <path/to/file>
```

* Discard changes on staged files

```
$ git checkout -- <path/to/file>
```

### Stash

Stash the changes in a dirty working directory away

* Move changes in current branch to another branch

```
$ git stash
$ git checkout <branch_name>
$ git stash pop
```

* Move changes in current branch to a new branch

```
$ git stash
$ git stash branch <branchname> [<stash>]
```

Its effect is the same as the effect of:

```
$ git stash
$ git checkout -b xxx
$ git stash pop
```
* Interrupted workflow

When you are in the middle of something, your boss comes in and demands that you fix something immediately.

```
... hack hack hack ...
$ git stash
... edit emergency fix ...
$ git commit -a -m "Fix in a hurry"
$ git stash pop
... continue hacking ...
```

## Branching model

![alt text](http://nvie.com/img/git-model@2x.png "Branching model")


## References

* [git - the simple guide](http://rogerdudler.github.io/git-guide/index.html)
* [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
* [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
