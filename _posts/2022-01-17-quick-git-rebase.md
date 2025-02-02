---
title: Quick git rebase
subtitle: Use git rebase to update your feature branch from main and prevent merge conflicts.
author: Andrew Campbell
tags: [git]
thumbnail-img: assets/img/rebase/rebase_logo.png
#gh-repo: acampb/AzureVMSnapshots
#gh-badge: [star, fork, follow]
---

![rebase_logo](/assets/img/rebase/rebase_logo.png)

Using git from the command line has always been a bit intimidating, with `git rebase` being the most challenging for me. I've typically put off learning how to do it properly and just relied on `git merge` to bring feature branches up to date with main and resolve merge conflicts. I finally had some time to sit down and learn it, and of course it's really not that bad! I created this quick demo to show how simple it can be.

## Setup the repo

Start with creating a new directory, initializing it as a git repository (no need to push it to GitHub, we'll work locally), create a new text file, and open the directory in VSCode.

```shell
mkdir rebase_demo
cd rebase_demo
git init
touch test.txt
code .
```

Add some text to the file, add it to git as a tracked file, and create the first commit directly to the `main` branch.

```shell
git add test.txt
git commit -m "Initial commit"
```

![rebase01](/assets/img/rebase/rebase01.png)

## Create a feature branch

Create a new branch named `branch1`. This will be based off of `main` at this point in time.

```shell
git checkout -b branch1
```

Make some additional edits to `test.txt` and commit them to `branch1`
![rebase02](/assets/img/rebase/rebase02.png)

## Create the merge conflict

Now we are going to intentially create a merge conflict. We'll do this by creating a new branch based off of `main`, making changes to our file there, and merging those changes into `main` ahead of our changes in `branch1`.

```shell
git checkout main
git checkout -b branch2
```

Modify the contents of `test.txt` and commit the changes to `branch2`.

![rebase03](/assets/img/rebase/rebase03.png)

Merge the changes from `branch2` into `main`. If you keep the `test.txt` file open in your VSCode editor pane you should be able to see the contents of the file changing as you switch back to the `main` branch, and after the merge command completes.

```shell
git checkout main
git merge branch2
```

![rebase04](/assets/img/rebase/rebase04.png)

## Rebase the feature branch

Switch back to `branch1` and we can now run `git rebase` to update the content of the file with the changes now in `main`.

```shell
git checkout branch1
git rebase main
```

![rebase05](/assets/img/rebase/rebase05.png)

We will need to decide which lines or changes to keep within the file. Once completed we commit our changes to our branch and then complete the `rebase`

```shell
git add test.txt
git commit -m "message"
git rebase --continue
```

![rebase06](/assets/img/rebase/rebase06.png)

With our feature branch successfully rebased we can switch back to `main` and merge our changes without conflicts.

![rebase07](/assets/img/rebase/rebase07.png)
