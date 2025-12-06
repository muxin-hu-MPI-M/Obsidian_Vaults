---
tags:
  - git_lab
---
## Version control
### Why version Control
- Programs consist of many folders and files.
- Code is moved between files.
- Who changed what when, and why?
- Which change introduced a bug?
- Check other people’s code before using it.
- Try some crazy idea, but then return to working version.
### What is Version Control
- track changes of many files in a folder hierarchy. changes can be undone (ad infinitum)
- many users can work in parallel and create different versions
- Different versions can be merged easily
- Authorship can be traced and verified
- Data integrity is ensured
- types:
	- **Centralised:** Version history is stored only on a central server, clients communicate changes (CVS, SVN, …)
	- **Distributed:** Every user has the complete project history, users communicate changes p2p or via a central server (Git, Mercurial, …)
### History
CVS: a centralised client-server model to RCS. A successor occasionally still used today

### Overview
- git:
	- design goals:
		- **Speed**: “patching should take no more than three seconds”
	    - Take CVS as an example of what **not** to do
	    - Support a **distributed** workflow; branching and merging should be easy
	    - Include **very strong safeguards against corruption**, either accidental or malicious.

## Learning Git

- [Software Carpenters](https://carpentries.org/) [“Version Control with Git”](https://swcarpentry.github.io/git-novice/)
- [Oh My Git!](https://ohmygit.org/) learning game
- [Pro Git Book](https://git-scm.com/book/en/v2)
- [Git documentation](https://git-scm.com/docs)

## Installing Git

- [https://git-scm.com/downloads](https://git-scm.com/downloads)
- **Linux:** Allmost all Linux distributions have git:  
    `apt install git`, or `dnf install git`, or …
- **Mac:** Contained in [Xcode](https://developer.apple.com/xcode/), or use [Homebrew](https://brew.sh/):  
    `brew install git`
- **Windows:** [https://gitforwindows.org/](https://gitforwindows.org/) (git + shell)  
    or use [Git in Windows Subsystem for Linux (WSL)](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-git)

## First steps

Git is a command line programm. Open a _terminal_ or _console_ and run:

```
$ git version
git version 2.47.3
```

Type the things after the `$` and press `Enter`.  
Lines without `$` represent the output of the command.

You can also try:

```
$ git help
```

On first use you should tell Git who you are:

```
$ git config --global user.name "Max Meier"
$ git config --global user.email "max@meier.com"
$ git config --global init.defaultBranch main
```

```
$ git config --list
user.name=Max Meier
user.email=max@meier.com
init.defaultbranch=main
```

`--global` sets options for _all_ your projects. Without this flag or with  
`--local` options are valid for the current project.

**Take home**

Git commands have the form:  
`git [command] [options] [argument(s)]`

# Using Git alone and locally

## Init new project & status

Create a new directory and go into it:

```
$ mkdir basic-git
$ cd basic-git
```

Now check the status:

```
$ git status
fatal: not a git repository (or any of the parent directories): .git
```

We first need to **initialize** Git version tracking!

```
$ git init
Initialized empty Git repository in /builds/weisse/git-school/basic-git/.git/
```

```
$ ls -la
total 12
drwxr-xr-x 3 root root 4096 Dec  1 11:25 .
drwxrwxrwx 7 root root 4096 Dec  1 11:25 ..
drwxr-xr-x 7 root root 4096 Dec  1 11:25 .git
```

Check status again:

```
$ git status
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

## First Commit

Create a file `my-first-file.md` with some content using a text editor or with

```
$ echo "Hello, world!" > my-first-file.md
```

Check status again

```
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    my-first-file.md

nothing added to commit but untracked files present (use "git add" to track)
```

### The two stages of a commit

![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/git-staging-area.svg)

> [!Attention]
> 1. changes are **added** to the ==**staging area**== (also called _index_) by `git add $FILE_NAME`
> 2. the staged changes are **committed** to the ==**repository**== as a new revision by `git commit -m 'your comment'`

Add the file to the staging area

```
$ git add my-first-file.md
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
    new file:   my-first-file.md
```

Now commit to the repository

```
$ git commit -m 'my first commit'
[main (root-commit) 337c8dc] my first commit
 1 file changed, 1 insertion(+)
 create mode 100644 my-first-file.md
```

Check status again

```
$ git status
On branch main
nothing to commit, working tree clean
```

Show the history

```
$ git log
commit 337c8dc831b445f0dac283c57646bf041e66d617
Author: Max Meier <max@meier.com>
Date:   Mon Dec 1 11:25:35 2025 +0000

    my first commit
```

## First changes

Edit the file `my-first-file.md` and insert two lines at the top. Use a text editor or

```
$ sed -i '1i # Opening\n' my-first-file.md
```

Check the status

```
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   my-first-file.md

no changes added to commit (use "git add" and/or "git commit -a")
```

What was changed?

```
$ git diff
diff --git a/my-first-file.md b/my-first-file.md
index af5626b..632c555 100644
--- a/my-first-file.md
+++ b/my-first-file.md
@@ -1 +1,3 @@
+# Opening
+
 Hello, world!
```

Commit the changes to the repository

```
$ git add .
$ git commit -m 'add a heading'
[main 5b9c68d] add a heading
 1 file changed, 2 insertions(+)
```

Check the status

```
$ git status
On branch main
nothing to commit, working tree clean
```

Edit the file again and add a few lines at the end …

```
$ sed -i '$ a  \\n# Working with git\n\nGit is fun!\n' my-first-file.md
```

Show the difference

```
$ git diff
diff --git a/my-first-file.md b/my-first-file.md
index 632c555..d8a9cdf 100644
--- a/my-first-file.md
+++ b/my-first-file.md
@@ -1,3 +1,8 @@
 # Opening
 
 Hello, world!
+
+# Working with git
+
+Git is fun!
+
```

==Add to staging and commit in one command==

```
$ git commit -a -m 'start a new section'
[main d717ab3] start a new section
 1 file changed, 5 insertions(+)
```

This does not work for adding new files, only for changes of existing.

Show the history in a compact form

```
$ git log --oneline
d717ab3 start a new section
5b9c68d add a heading
337c8dc my first commit
```

More verbose history

```
$ git log --stat
commit d717ab332e920d82e29a6017a89779e0b0257e20
Author: Max Meier <max@meier.com>
Date:   Mon Dec 1 11:25:35 2025 +0000

    start a new section

 my-first-file.md | 5 +++++
 1 file changed, 5 insertions(+)

commit 5b9c68dfdff41a26794ae96ee68b38c9f9f1980c
Author: Max Meier <max@meier.com>
Date:   Mon Dec 1 11:25:35 2025 +0000

    add a heading

 my-first-file.md | 2 ++
 1 file changed, 2 insertions(+)

commit 337c8dc831b445f0dac283c57646bf041e66d617
Author: Max Meier <max@meier.com>
Date:   Mon Dec 1 11:25:35 2025 +0000

    my first commit

 my-first-file.md | 1 +
 1 file changed, 1 insertion(+)
```

## Good commit messages

[![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/git_commit.png)](https://imgs.xkcd.com/comics/git_commit.png)

- The first line (the subject) of the commit message should give a **concise summary** of the changes
- After an empty line the commit message can describe more details of the change ==(`git commit` without the `-m` will open an editor)
- Good examples of commit messages for can be found, e.g., in the repositories of the Linux kernel, [https://git.kernel.org](https://git.kernel.org/) or [https://github.com/torvalds/linux](https://github.com/torvalds/linux)

## What we’ve learned so far

### ‼️ Basic Git commands

```
$ git status
$ git log [--oneline | --stat]
$ git diff

$ git add <file>...
$ git commit -m "What did I change?"

$ git init
$ git config
```

**Exercises**

Practise repository initialisation and simple edits and commits.

## Ignoring files

- Frequently, projects contain files whose history is uninteresting (automatically generated, temporary, editor backups, logs)
- Git ignores all files or patterns listed in **`.gitignore`**
- Ignored files can still be added and committed using `git add -f`

```
$ cat .gitignore
git-school_files/
git-school_cache/
git-school.html
/.quarto/
public/
basic-git/
intermediate-git/
intermediate-git-remote.git/
*~

**/*.quarto_ipynb
```

## Time travel

- A ==**_commit_ is a _snapshot_ of all files**
- Each commit has a unique _hash_, formed from all data and the parent hash ([Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree), “block chain”)
- _HEAD_ is the commit in the tree to which the next commit will be appended
![[Screenshot 2025-12-01 at 15.07.02.png |center]]

> [!Tip]
> **Hash functions**
> (Cryptographic) [hash functions](https://en.wikipedia.org/wiki/Hash_function) are “one-way” functions, with the following properties:
> - an arbitrary amount of input data is transformed to a fixed-size number (the _hash_)
> - any change of the input changes the output
> - the function is very hard to invert: it is very hard to generate input, that results in a given output (therefore bitcoin mining burns so gigantic amounts of energy)
> Common property of Merkle trees and block chains:
> - the previous hash _and_ the new data are used to calculate the next hash
> - a change in historical data therefore changes _all_ later hashes
>   **→ changes of history and data corruption can be detected!**

### ‼️ Compare commits

Compare with previous version `git diff HEAD~1`

```
$ git diff HEAD~1
diff --git a/my-first-file.md b/my-first-file.md
index 632c555..d8a9cdf 100644
--- a/my-first-file.md
+++ b/my-first-file.md
@@ -1,3 +1,8 @@
 # Opening
 
 Hello, world!
+
+# Working with git
+
+Git is fun!
+
```

Compare with version two commits before `git diff HEAD~2`

```
$ git diff HEAD~2
diff --git a/my-first-file.md b/my-first-file.md
index af5626b..d8a9cdf 100644
--- a/my-first-file.md
+++ b/my-first-file.md
@@ -1 +1,8 @@
+# Opening
+
 Hello, world!
+
+# Working with git
+
+Git is fun!
+
```

Compare two arbitrary commits 
first get the commit IDs by `git log --oneline`

```
$ first get the commits ID by git log --oneline
$ git diff 337c8dc 5b9c68d
diff --git a/my-first-file.md b/my-first-file.md
index af5626b..632c555 100644
--- a/my-first-file.md
+++ b/my-first-file.md
@@ -1 +1,3 @@
+# Opening
+
 Hello, world!
```

### ‼️ View old version temporarily

With `git checkout` we can inspect an earlier version:

```
$ git log --oneline
d717ab3 start a new section
5b9c68d add a heading
337c8dc my first commit
```

```
$ git checkout 337c8dc
Note: switching to '337c8dc'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 337c8dc my first commit
```

Here’s the content of the old version:

```
$ cat my-first-file.md
Hello, world!
```

With `git checkout main` we can ==***return***

```
$ git checkout main
Previous HEAD position was 337c8dc my first commit
Switched to branch 'main'
```

```
$ cat my-first-file.md
# Opening

Hello, world!

# Working with git

Git is fun!
```

### Restore an old version and work from it

With `git restore` we can retrieve an old version of one or many files:

```
$ git log --oneline
d717ab3 start a new section
5b9c68d add a heading
337c8dc my first commit
```

```
$ git restore -s 5b9c68d my-first-file.md
```

```
$ git status
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   my-first-file.md

no changes added to commit (use "git add" and/or "git commit -a")
```

> [!Attention]
> After restore the file is in the _old state_ and the corresponding changes are ==_not committed_==.

See the diff

```
$ git diff
diff --git a/my-first-file.md b/my-first-file.md
index d8a9cdf..632c555 100644
--- a/my-first-file.md
+++ b/my-first-file.md
@@ -1,8 +1,3 @@
 # Opening
 
 Hello, world!
-
-# Working with git
-
-Git is fun!
-
```

Edit the file again and add other section at the end …

```
$ sed -i '$ a  \\n# Learning Git\n\nSlowly we make progress...\n' my-first-file.md
```

```
$ git commit -am 'replaced the second section'
[main 7fd36e1] replaced the second section
 1 file changed, 2 insertions(+), 2 deletions(-)
$ git log --oneline
7fd36e1 replaced the second section
d717ab3 start a new section
5b9c68d add a heading
337c8dc my first commit
```

> [!Attention]
> So if restore to a ‘old’ version and then commit it, it will be a new version in the repository, adding top of the previously ‘latest’ version

### Undo a (faulty) commit

`git revert` creates a new commit that undoes a given old commit

```
$ git log --oneline
7fd36e1 replaced the second section
d717ab3 start a new section
5b9c68d add a heading
337c8dc my first commit
```

```
$ git show 5b9c68d
commit 5b9c68dfdff41a26794ae96ee68b38c9f9f1980c
Author: Max Meier <max@meier.com>
Date:   Mon Dec 1 11:25:35 2025 +0000

    add a heading

diff --git a/my-first-file.md b/my-first-file.md
index af5626b..632c555 100644
--- a/my-first-file.md
+++ b/my-first-file.md
@@ -1 +1,3 @@
+# Opening
+
 Hello, world!
```

Now the undo …

```
$ git revert -n 5b9c68d
Auto-merging my-first-file.md
```

```
$ git status
On branch main
You are currently reverting commit 5b9c68d.
  (all conflicts fixed: run "git revert --continue")
  (use "git revert --skip" to skip this patch)
  (use "git revert --abort" to cancel the revert operation)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    modified:   my-first-file.md
```

```
$ git diff --cached
diff --git a/my-first-file.md b/my-first-file.md
index 59520b7..1b4cc45 100644
--- a/my-first-file.md
+++ b/my-first-file.md
@@ -1,5 +1,3 @@
-# Opening
-
 Hello, world!
 
 # Learning Git
```

```
$ git commit -am 'undo 2nd commit'
[main 79d19de] undo 2nd commit
 1 file changed, 2 deletions(-)
```


## What we’ve learned so far

**Undo commands**

- `git checkout` to move to an old commit (temporarily)
- `git restore` to bring back files from old commits
- `git revert` to undo past (faulty) commits
- `git reset --hard` to completely reset to old commits (dangerous!)

**Git commands**

```
$ git status
$ git log [--oneline | --stat]
$ git diff
$ git show <commit>

$ git add <file>...
$ git commit -m "What did I change?"

$ git checkout <commit | branch>
$ git restore -s <commit> <file>...
$ git revert [-n] <commit>

$ git init
$ git config
```

# Branches

![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/branching-illustration@2x.png)

## Branching with `git branch`

![[Screenshot 2025-12-01 at 15.33.45.png |center]]

- Branches enable different versions / states of a project
- New features can first be developed and tested in a branch
- Team members can initially work in their own branches and merge later
- Remote branches on a server enable collaboration

Create a new branch

```
$ git branch my-experiment
```

List all branches

```
$ git branch -a
* main
  my-experiment
```

==Switch to the new branch==

> [!Attention]
> before switch to a new branch, you have to merge (add and commit) the changes in the current branch!!! (like you need to save the changes, or simply undo the changes)

```
$ git checkout my-experiment
Switched to branch 'my-experiment'
```

```
$ git status
On branch my-experiment
nothing to commit, working tree clean
```

Edit something and commit to the new branch

```
$ sed -i '1i # Introduction\n' my-first-file.md
$ git diff
diff --git a/my-first-file.md b/my-first-file.md
index 1b4cc45..b510e2d 100644
--- a/my-first-file.md
+++ b/my-first-file.md
@@ -1,3 +1,5 @@
+# Introduction
+
 Hello, world!
 
 # Learning Git
```

```
$ git commit -am 'try a new heading'
[my-experiment e335ac0] try a new heading
 1 file changed, 2 insertions(+)
```

```
$ git log --oneline --decorate
e335ac0 (HEAD -> my-experiment) try a new heading
79d19de (main) undo 2nd commit
7fd36e1 replaced the second section
d717ab3 start a new section
5b9c68d add a heading
337c8dc my first commit
```

## ‼️ Merging with `git merge`

![[Screenshot 2025-12-01 at 15.41.25.png |center]]
- Once development in a branch is complete, the changes can be merged into the main branch
- `git merge` is very intelligent and usually works automatically
- _Don’t be afraid of merges!_

Go back to main branch

```
$ git checkout main
Switched to branch 'main'
```

Merge changes from `my-experiment` into `main`

```
$ git merge my-experiment
Updating 79d19de..e335ac0
Fast-forward
 my-first-file.md | 2 ++
 1 file changed, 2 insertions(+)
```

View log

```
$ git log --oneline --decorate
e335ac0 (HEAD -> main, my-experiment) try a new heading
79d19de undo 2nd commit
7fd36e1 replaced the second section
d717ab3 start a new section
5b9c68d add a heading
337c8dc my first commit
```

## Resolving conflicts

Lets create a conflict: Add a line in a new branch …

```
$ git checkout -b trouble
Switched to a new branch 'trouble'
```

```
$ sed -i '$ a # Summary' my-first-file.md
$ git commit -am 'add summary'
[trouble f9353b9] add summary
 1 file changed, 1 insertion(+)
```

… and add a different line in the main branch

```
$ git checkout main
Switched to branch 'main'
```

```
$ sed -i '$ a # Advanced features' my-first-file.md
$ git commit -am 'start 3rd section'
[main 2cae458] start 3rd section
 1 file changed, 1 insertion(+)
```

Try a merge

```
$ git merge trouble
Auto-merging my-first-file.md
CONFLICT (content): Merge conflict in my-first-file.md
Automatic merge failed; fix conflicts and then commit the result.
```

```
$ git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
    both modified:   my-first-file.md

no changes added to commit (use "git add" and/or "git commit -a")
```

```
$ cat my-first-file.md
# Introduction

Hello, world!

# Learning Git

Slowly we make progress...

<<<<<<< HEAD
# Advanced features
=======
# Summary
>>>>>>> trouble
```

Edit the file and fix the problems between `<<<` and `>>>`

```
$ sed -i -e '$a# Conclusions' -e 9,13d my-first-file.md
```

Then finish the merge commit

```
$ git commit -am 'fix conflict and merge'
[main 372b7a2] fix conflict and merge
```

```
$ git status
On branch main
nothing to commit, working tree clean
```

```
$ git log --graph --oneline --decorate
*   372b7a2 (HEAD -> main) fix conflict and merge
|\  
| * f9353b9 (trouble) add summary
* | 2cae458 start 3rd section
|/  
* e335ac0 (my-experiment) try a new heading
* 79d19de undo 2nd commit
* 7fd36e1 replaced the second section
* d717ab3 start a new section
* 5b9c68d add a heading
* 337c8dc my first commit
```

## Housekeeping

List branches

```
$ git branch -a
* main
  my-experiment
  trouble
```

Delete merged branches

```
$ git branch -d my-experiment trouble
Deleted branch my-experiment (was e335ac0).
Deleted branch trouble (was f9353b9).
```

List again

```
$ git branch -a
* main
```

## What we’ve learned so far

**Git commands**

```
$ git status
$ git log [--oneline | --stat | --graph | --decorate]
$ git diff
$ git show <commit>

$ git add <file>...
$ git commit [-a] -m "What did I change?"

$ git branch [-a | -d] <branch>
$ git checkout [-b] <branch>
$ git merge

$ git checkout <commit | branch>
$ git restore -s <commit> <file>...
$ git revert [-n] <commit>

$ git init
$ git config
```

**Exercises**

- Create branches and merge back into main. Provoke a merge conflict and resolve it.

# Git for collaboration

## How to collaborate using Git

- Even if you work alone, Git is extremely useful (texts, documentation, code, configuration, etc.).
    
- Traditionally, [changes can be exchanged as emails](https://git-send-email.io/):  
    **`git format-patch`** and **`git am`** (‘apply-mail’)
    
- Today, centralised (or distributed) web or SSH servers are commonly used to communicate commits and as a backup
    

## Git servers

- Shared folder, file server or SSH server that all team members have access to
    
- [Github](https://github.com/): Very well-known, _proprietary_ web platform for Git projects; owned by Microsoft.
    
- [Gitlab](https://about.gitlab.com/): Well-known, _open-source_ web platform for Git projects; self-hosting possible.
    
- [Forgejo](https://forgejo.org/): _Open-source_ web platform for self-hosting; less bloated than Gitlab. Basis for free Github alternative [Codeberg](https://codeberg.org/).
    
- [Sourcehut](https://sourcehut.org/): _Open-source_ web platform “for hackers”; free Github alternative; self-hosting possible.
    
- [Gitolite](https://gitolite.com/gitolite/): Simple SSH server with clever scripts for managing projects and users
    
- [Radicle](https://radicle.xyz/): New _open-source_ peer-to-peer Git collaboration system.
    

## Preparations

### Creating an Gitlab account

- For this course you can sign-up at:  
    [https://gitlab.mpim-bonn.mpg.de/](https://gitlab.mpim-bonn.mpg.de/)

![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/mpim-gitlab-qr.svg)

 

- Please use a strong password of at least 12 characters.
    
- After approval, you can login under the “Standard” tab.
    

### Working with SSH

- [SSH (Secure Shell)](https://en.wikipedia.org/wiki/Secure_Shell) is an encrypted protocol and a program to connect to remote computers.
- It can be used to open a shell on a remote computer (`ssh`), copy data between computers (`scp`), and route internet traffic.
- For authentication, SSH can use [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography).
- [Gitlab Documentation for SSH](https://docs.gitlab.com/user/ssh/)

- If you don’t have one, create a public-private key pair with:  
    
    ```
    cd ~/.ssh
    ssh-keygen -t ed25519
    ```
    
    Specify a **filename** and a **passphrase**, when asked.
    
- This will create two files, for example:
    
    ```
    gitlab-bn       # private key (keep secret!)
    gitlab-bn.pub   # public key (can be shared)
    ```
    
- _Never share or upload the private key!_
    

- In Gitlab go to User settings → SSH Keys, and “Add new key”
    
    - Copy contents of your public key file (.pub)  
        into the “Key” field
    - fill the “Title” field
    - set “Usage type” to “Authentication & signing”.
- On your computer, configure SSH to use a particular key for a given host: Open `~/.ssh/config` and add the lines:
    
    ```
    Host gitlab.mpim-bonn.mpg.de
        User git
        Hostname gitlab.mpim-bonn.mpg.de
        IdentityFile ~/.ssh/gitlab-bn
    ```
    
- Now you can clone, pull and push to your projects over SSH.
    

## Working with Git servers

- Git servers host special branches, called **remote branches**, or just **remotes**
- New commands: `git clone`, `git push`, `git fetch` and `git pull = git fetch + git merge`

![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/git-areas.png)

Creating projects on a Git server depends on the server type.

Here we create a remote on Gitlab using push:

```
$ git remote add origin git@gitlab.mpim-bonn.mpg.de:weisse/basic-git.git
$ git push --set-upstream origin main
```

```
Enumerating objects: 27, done.
Counting objects: 100% (27/27), done.
Delta compression using up to 4 threads
Compressing objects: 100% (16/16), done.
Writing objects: 100% (27/27), 2.19 KiB | 1.09 MiB/s, done.
Total 27 (delta 6), reused 0 (delta 0), pack-reused 0 (from 0)
remote: 
remote: 
remote: The private project weisse/basic-git was successfully created.
remote: 
remote: To configure the remote, run:
remote:   git remote add origin git@gitlab.mpim-bonn.mpg.de:weisse/basic-git.git
remote: 
remote: To view the project, visit:
remote:   https://gitlab.mpim-bonn.mpg.de/weisse/basic-git
remote: 
remote: 
remote: 
To gitlab.mpim-bonn.mpg.de:weisse/basic-git.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

We can also login on the server, create a project, and then use `git clone` to retrieve a local copy:

![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/gitlab-create-1.png)![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/gitlab-create-2.png)![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/gitlab-create-3.png)

## Cloning projects

- The command `git clone` is used for the _first_ retrieval of a remote project.
- `git clone` retrieves a copy of the complete project history, information about all branches, and does a checkout of the main branch.
- The data can be transferred over different protocols, in particular, SSH, HTTPS and local file access.

Clone the new project over SSH:

```
$ git clone git@gitlab.mpim-bonn.mpg.de:weisse/intermediate-git.git
Cloning into 'intermediate-git'...
```

Clone from a (bare) repo in the local file system:

```
$ git clone intermediate-git-remote.git intermediate-git
Cloning into 'intermediate-git'...
done.
```

```
$ cd intermediate-git
$ ls -la
total 20
drwxr-xr-x 3 root root 4096 Dec  1 11:25 .
drwxrwxrwx 9 root root 4096 Dec  1 11:25 ..
drwxr-xr-x 8 root root 4096 Dec  1 11:25 .git
-rw-r--r-- 1 root root 6164 Dec  1 11:25 README.md
```

```
$ git log --oneline --decorate
6c763ed (HEAD -> main, origin/main, origin/HEAD) Initial commit
```

Working over SSH works best with public/private key authentication. Usually this requires uploading your public SSH key to the Git server.

## Working with remotes

Before starting to work, check for updates on the remote side:

```
$ git fetch
$ git merge
Already up to date.
```

This is equivalent to

```
$ git pull
Already up to date.
```

Now edit and commit locally

```
$ sed -i '1i # Working with remotes\n\nThis is a sample project to illustrate the interaction with remote branches.\n' README.md
$ sed -i '4,$d' README.md
```

```
$ git commit -am 'update README'
[main 06bb8a3] update README
 1 file changed, 2 insertions(+), 92 deletions(-)
```

Check the log

```
$ git log --oneline --decorate
06bb8a3 (HEAD -> main) update README
6c763ed (origin/main, origin/HEAD) Initial commit
```

Check status

```
$ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

Upload the local changes to the server

```
$ git push
To /builds/weisse/git-school/intermediate-git-remote.git
   6c763ed..06bb8a3  main -> main
```

A push can fail if someone else pushed before you. You’ll then need to fetch and merge (=pull), before you can push. Don’t be afraid!

## Typical work day

```
$ git checkout branch-name
$ git pull [--all]

$ # edit files ...
$ git add .
$ git commit -m "changed feature A"

$ # edit files ...
$ git add .
$ git commit -m "updated documentation for feature A"

$ git push
```

- Local branches each have their own assigned remote
- Before a `git push`, you should be sure that everything is correct and working properly. You are showing your work to others, and any subsequent changes will affect everyone!

## Managing remotes

With `git remote` we can view and configure remote branches.

```
$ git remote -v
origin  git@gitlab.mpim-bonn.mpg.de:weisse/intermediate-git.git (fetch)
origin  git@gitlab.mpim-bonn.mpg.de:weisse/intermediate-git.git (push)
```

```
$ git remote add ...
$ git remote remove ...
$ git remote rename ...
$ git remote set-url ...
```

See also

```
$ git help remote
```

## What we’ve learned so far

**Git commands**

```
$ git status
$ git log [--oneline | --stat | --graph | --decorate]
$ git diff

$ git add <file>...
$ git commit [-a] -m "What did I change?"

$ git branch [-a | -d] <branch>
$ git checkout [-b] <branch>
$ git merge

$ git checkout <commit | branch>
$ git restore -s <commit> <file>...
$ git revert [-n] <commit>

$ git pull (= git fetch + git merge)
$ git push

$ git init
$ git clone
$ git config
$ git remote
```

# Gitlab

## What is Gitlab?

- Server for storing Git repositories
- Project management tool
- Integrated development environment (IDE)
- Issue tracker and Kanban board
- Compile and deploy machine (CI/CD)
- Web server
- Wiki
- Repository for publishing software
- …

### Screenshot project view

![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/gitlab-create-3.png)

## Where can you use Gitlab?

- At [Gitlab.com](https://about.gitlab.com/pricing/)
- On your [own server](https://about.gitlab.com/install/)
- At many universities and institutes …

## Operation

- If you know Git basics, Gitlab it rather easy to use
- Most pages have explanations and links to documentation
- The [documentation](https://docs.gitlab.com/) is quite good and detailed
- First, check your profile and **upload public SSH keys**
- Gitlab has granular access rights: read-only, write-only on individual branches, etc.
- Collaboration takes place via branches and **merge requests** (this differs from Github, which works with forks and pull requests)

## User roles

- Use Manage → Members to invite people to your project.
- You can assign different [Roles with different permissions](https://docs.gitlab.com/user/permissions/):

- **Owner:** can do everything, also _delete_ the project
- **Maintainer:** Almost like owner, but _cannot delete_ project
- **Developer:** Can read & write (push) to non-protected branches (≠ main)
- **Reporter:** Can read & report issues
- **Planner:** Can manage issues and plan milestones
- **Guest:** Can only read and comment on issues

## Edit and commit

- From the project view you can navigate to every file, edit and commit changes.
- Gitlab has two edit options (press the “Edit” button):
    - **Edit single file:** opens the current file in a simple editor
    - **Open in Web IDE:** opens the entire project in a [VS Code](https://vscodium.com/) like IDE
- When ready, you can commit to the current or a new branch (depends on role).

## Merge requests

- Whenever you create or push to a new branch, Gitlab will offer to create a **Merge request**.
- Merge requests ask maintainers to merge a branch into the main branch (or any other).
- Merge requests can be commented, linked to issues and assigned to people.
- Merge requests can be accepted or rejected.
- **It’s a way to communicate and review changes.**

## CI/CD

- [CI/CD](https://en.wikipedia.org/wiki/CI/CD) = continuous integration and continuous delivery
    
- The Gitlab server is used to test, compile, publish and run software automatically after commits.
    
- Examples:
    
    - Compile LaTeXLATE​X documents automatically.
    - Test & compile a software project and publish the binary.
    - Perform security audits of software.
    - Build a static web page and publish it.
    - …

### `.gitlab-ci.yml`

- The CI/CD jobs for a project are configured in a special file: `.gitlab-ci.yml`
    
- It describes the required software (docker image), the stages of the ‘pipeline’ and the steps to perform, when certain conditions are fulfilled.
    
- Gitlab has a built-in ‘Pipeline editor’ setup and validate the CI file.
    
- Pipelines are executed on special (virtual) servers called ‘Runners’
    

### Example: Compile a TeX project

**.gitlab-ci.yml**

```
image: registry.gitlab.com/islandoftex/images/texlive:latest

variables:
  MAIN_TEX_NAME: abschlussarbeit

latexmk:
  stage: build
  script:
    - latexmk -pdf $MAIN_TEX_NAME
  artifacts:
    paths:
      - $MAIN_TEX_NAME.pdf
```

### Example: Test and compile a Go programm

**.gitlab-ci.yml**

```
image: golang:1.25.4-alpine

stages:
  - test
  - build

test-go:
  stage: test
  script:
    - go test -v

build-go:
  stage: build
  script:
    - go build
  artifacts:
    paths:
      - fibonacci
  only:
    - main
```

## Pages

- Gitlab (like Github) contains a web server for hosting static web pages: [Gitlab Pages](https://docs.gitlab.com/user/project/pages/)
    
- It can be used to host documentation for a project, or HTML presentations like this one, etc…
    
- CI/CD can be used to build the web page (e.g. from Markdown sources), and publish it on Pages.
    

### Example: Build and host a HTML presentation

- [Quarto](https://quarto.org/) is an open-source scientific and technical publishing system, which translates [Markdown](https://en.wikipedia.org/wiki/Markdown) input into many different output formats, including [Revealjs](https://revealjs.com/) presentations.

**.gitlab-ci.yml**

```
image: ghcr.io/quarto-dev/quarto:1.8.26

pages:
  script:
    - quarto check
    - quarto render slides.qmd --output-dir public -o index.html
  artifacts:
    paths:
      - public
  only:
    - main
```

# To be continued …

## Additional Git commands

- You may want to highlight certain commits:  
    **`git tag -a v1.1.0 -m "version 1.1.0"`**
    
- To ensure authenticity, commits and tags can be cryptographically signed with GPG, SSH-Keys, or s/mime:  
    **`git commit -S -m "Signed commit"`**  
    **`git tag -a v1.1.0 -s -m "version 1.1.0"`**
    
- Git is a file system that can be checked and cleaned up:  
    **`git fsck`** (_file system check_) and **`git gc`** (_garbage collect_)  
    Git servers do this automatically from time to time.
    
- For further commands, see [**`git help`**](https://git-scm.com/docs)
    

## Large files

- Large binary files (images, data, etc.) are difficult to describe using changes (diffs) and slowdown git operations. _Avoid committing large binary files._
- Better: Use **`git lfs`**. Git then only monitors checksums and meta data, and the server stores and manages the content.
- Git LFS is an extension you need to install, and the Git server needs to support LFS. See: [https://git-lfs.com/](https://git-lfs.com/)

## Diff and merge tools

- Git can call external programs for diff and merge ([`git difftool`](https://git-scm.com/docs/git-difftool))
- [Meld](https://meldmerge.org/) is a nice graphical diff tool for files and folders.
- Source code can differ slightly in line breaks, spaces, order of (some) expressions, but still mean the same thing.
- Git does not understand the syntax and semantics of code and can detect unneccessary merge conflicts.
- [difftastic](https://difftastic.wilfred.me.uk/) is a new tool that understands program code in many languages and shows better diffs.
- [Mergiraf](https://mergiraf.org/) is a new tool that—like difftastic—understands source code and resolves more merge conflicts automatically.

## SSH Extras

- Signing commits and tags with SSH
    
- Retrieve a users public key from Gitlab:  
    
    ```
    curl -s "https://<gitlab server>/<username>.keys"
    ```
    
- Encryption with SSH-Keys and [age](https://github.com/FiloSottile/age)
    

Slides of this presentation:  
[https://git-school-a30a23.docs.mpim-bonn.mpg.de/](https://git-school-a30a23.docs.mpim-bonn.mpg.de/) or  
[https://s.gwdg.de/Vg85yq](https://s.gwdg.de/Vg85yq)

![](https://git-school-a30a23.docs.mpim-bonn.mpg.de/images/slides-qr.svg)

[](https://git-school-a30a23.docs.mpim-bonn.mpg.de/git-school.html#)

Overview Started in 2005 by Linus Torvalds as a VCS for the Linux kernel (after the proprietary BitKeeper changed its license). Design goals : Speed : “patching should take no more than three seconds” Take CVS as an example of what not to do Support a distributed workflow; branching and merging should be easy Include very strong safeguards against corruption , either accidental or malicious. Free software: licensed under GPL 2

1.   
    Slides
2.   
    Tools
3.   
    Close

- Working with Git and Gitlab/Github
- Why version control?
- Writing papers with colleagues
- My latest article...
- Observation Manual...
- Writing programs
- What is version control?
- Features
- Types of version control systems
- Short history
- Git
- Overview
- Learning Git
- Installing Git
- First steps
- On first use you...
- Using Git alone and locally
- Init new project & status
- Check status again:...
- First Commit
- The two stages of a commit
- Add the file to the...
- Check status again...
- First changes
- What was changed?...
- Commit the changes...
- Edit the file again...
- Add to staging and...
- More verbose history...
- Good commit messages
- What we’ve learned so far
- Ignoring files
- Time travel
- Hash functions ...
- Compare commits
- Compare with version...
- Compare two arbitrary...
- View old version temporarily
- Here’s the content...
- Restore an old version and work from it
- See the diff $...
- Edit the file again...
- Undo a (faulty) commit
- Now the undo … ...
- What we’ve learned so far
- Git commands $...
- Branches
- Branching with git branch
- Create a new branch...
- Edit something and...
- Merging with git merge
- Go back to main branch...
- Resolving conflicts
- Try a merge $ git...
- Edit the file and...
- Housekeeping
- What we’ve learned so far
- Git for collaboration
- How to collaborate using Git
- Git servers
- Preparations
- Working with SSH
- If you don’t have...
- In Gitlab go to User...
- Working with Git servers
- Creating projects...
- We can also login...
- Cloning projects
- Clone the new project...
- Working with remotes
- Check the log $...
- Typical work day
- Managing remotes
- What we’ve learned so far
- Gitlab
- What is Gitlab?
- Screenshot project view
- Where can you use Gitlab?
- Operation
- User roles
- Edit and commit
- Merge requests
- CI/CD
- .gitlab-ci.yml
- Example: Compile a TeX project
- Example: Test and compile a Go programm
- Pages
- Example: Build and host a HTML presentation
- To be continued …
- Additional Git commands
- Large files
- Diff and merge tools
- SSH Extras
- Slides of this presentation:...