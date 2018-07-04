---
title: Version Control
linktitle: Version Control
description: Become familiar with version control and github.
date: 2013-07-01
publishdate: 2013-07-01
categories: [workflow]
keywords: [git,version control]
authors: [Karel L .Kubat]
menu:
  docs:
    parent: "workflow"
    weight: 10
weight: 10
sections_weight: 10
draft: false
aliases: [/git/,/vcs/]
toc: true
---

{{% note %}}
You will need [Git installed](https://git-scm.com/downloads) to run this tutorial and have a [github](https://github.com) account.
{{% /note %}}

*I spend 10% of my day coding, 30% automated testing, and 60% unraveling and amending my git history.*

-- You in a few weeks.



## What is VCS

VCS is simply the administration of different versions of some object, such as a code repository, a book, website or other
collection of information. You might order your code by creating a directory with files, named:

* Awesomecode.workingfile
* Awesomecode.final
* Awesomecode.final2
* Awesomecode.final2.final 


However this is obviously a crap solution. Instead we use a version control manager. This program will monitor checkpoints
(saves or commits) and store the changes in the file, while keeping the originals as well. This will ensure
that you have a history of all changes made to the project.

Git is the best and most used program 
to handle this. I am not going to describe git in full, while far better [resources](https://try.github.io/) are out there. 

## Why git

The need for VCS is not immediately apparent when working on a project by yourself. It might seem like a cumbersome CLI to
save files and possibly push them to the cloud. However when working with a larger team on the same codebase, there is no 
way without it. 

When working on a file, visualize that you are working on a branch from the main file (master). When your colleague also
works on that specific file, git will recognise conflicting lines. Both of you might have edited the same line of code.
If you edited it in exactly the same way no conflicts arise, and git will go it's merry way. If conflicts are present, 
git will ask you to resolve these (decide which lines to keep).

VCS is also convenient to figure out who edited what line, and ensure everyone is working on the newest version.



## Git workflow

After finishing the tutorial, you should be able to use git (I don't expect you to understand it completely, I don't either.)
In general your workflow will look like this

### Checkout/clone
We want to start working on a certain part of the codebase, for example fixing this typoi. First check out the repository
to get a local copy.

If it is the first time you start working on this project, you need to clone the repository

```
git clone https://github.com/CIP-NL/Documentation
```

If you have worked on it before, you can update it to the latest version

```
git checkout
```

Now you have a copy of all the files, which you can savely edit locally.

## Commit

Lets fix the typo from above. Open Documentation/content/workflow/git.md in your text editor and fix the typo.

After fixing the typo, commit your changes. This will tell git to make a save point which you can restore later.

``` 
git commit -m fixed typo: typoi
```

The flag m stands for message, which is a short message to identify what you changed later on.

Make sure your commits are a small as possible. Every commit should however result in a passing build. So if you
need to refactor a class method name, also edit all calls to that method to reflect the changes in the name.
You need to be able to roll back to a previous version on a per commit basis.

## Push

After finishing up working on the project, you can push the changes to the remote repository, in our
case github.com. 

``` 
git push origin master
```

origin stands for the repository, and master for the branch you are pushing to. You could also set a different repository location,
for example: 

``` 
git push mybasement master
```

{{% note %}}
This push to master should fail, because github is set to protect master.
{{% /note %}}

Although you won't be using that often. More likely you will push to a different branch. In general we use the branch development
when working on multiple small features. A large feature gets its own branch.

``` 
git push origin development
```
{{% note %}}
This push should succeed, because development is not protected.
{{% /note %}}

## Pull request

You noticed the push to master failed. Pushing to development may also fail in a different project, if that branch is protected.
Instead of directly pushing, you create a pull request. 

Create a new branch while checking out master/development.

``` 
git checkout -b [name_of_your_new_branch]
```

Do your stuff to the code.

``` 
git push origin [name_of_your_new_branch]
```

Now navigate to the repository, and create a pull request to master/development. This means that you ask the owner of
the master/development to pull your changes into their branch.

In our case, a pull request means that you write a short description of the changes you made,
and then have the code chang reviewed by a colleague.

After the PR is accepted, your changed are merged.


## Merge

When multiple people are working on the same codebase, conflicts might occur. To minimize conflicts, make sure
to merge before creating a PR.

Whenever you want to update your fork with the latest upstream changes, you'll need 
to first fetch the upstream repo's branches and latest commits to bring them into your repository.

```
# Fetch upstream master and merge with your repo's master branch
git fetch upstream
git checkout master
git merge upstream/master

# If there were any new commits, rebase your development branch
git checkout newfeature
git rebase master
```

## Links

Some convenient links:

* [git worklow](https://gist.github.com/Chaser324/ce0505fbed06b947d962)