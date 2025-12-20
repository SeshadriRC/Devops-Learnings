**Notes**

- Whenever we initialized a git repo **git init**, we need to commit it, then only if we give command **git branch** branch will get visible

- [Git log](#git-log)
- [git restore](#git-restore)


# Kodekloud

- [Fork a Git Repository](#fork-a-git-repository)

---
# git log

1. If you want to find commit id using commit message through git log command ?

```
git log --grep="commit message text"
git show commit id
```

---
# git restore

1. suppose you accidentally deleted file in branch, then give git restore
   
```
user@LAPTOP-QMBUJPPJ MINGW64 /Devops-Learnings (main)
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    Resume-topics.md

no changes added to commit (use "git add" and/or "git commit -a")

user@LAPTOP-QMBUJPPJ MINGW64 /Devops-Learnings (main)
$ git restore Resume-topics.md

user@LAPTOP-QMBUJPPJ MINGW64 /Devops-Learnings (main)
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

```

2. Multiple files deleted, but restore only single file

```
user@LAPTOP-QMBUJPPJ MINGW64 /Devops-Learnings (main)
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    Helm/helm-commands.md
        deleted:    Linux/Yum.md
        deleted:    Terraform/Topics.md

no changes added to commit (use "git add" and/or "git commit -a")

user@LAPTOP-QMBUJPPJ MINGW64 /Devops-Learnings (main)
$ git restore Terraform/Topics.md

user@LAPTOP-QMBUJPPJ MINGW64 /Devops-Learnings (main)
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    Helm/helm-commands.md
        deleted:    Linux/Yum.md

no changes added to commit (use "git add" and/or "git commit -a")

user@LAPTOP-QMBUJPPJ MINGW64 /Devops-Learnings (main)
$ git restore .

user@LAPTOP-QMBUJPPJ MINGW64 /Devops-Learnings (main)
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

user@LAPTOP-QMBUJPPJ MINGW64 /Devops-Learnings (main)

```

# Fork a git repository

```
go to git ui , navigate to sarah repository and fork it
```
