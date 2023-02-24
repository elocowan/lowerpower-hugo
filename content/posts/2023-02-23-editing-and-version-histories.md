---
title: Editing and Version Histories
date: 2023-02-23T19:03:00Z
---

I would like to start editing the posts that I've got on the site now.
But I don't want to lose any information about what they are/were like in their original state.
So I think I need to use and understand git at this point.

For context, I haven't initiated git on this project yet.
And I definitely haven't pushed anything to github.
And I'm even farther away from publishing anything...

But I am starting to think about version controls, I guess just because I'm weird like that?
I'm 'weird like that' in the sense that I want to be able to see how any of these documents change over time.
'Weird' because I'm not sure I'll ever look at the history, but I would like to be able to.
I don't know WHY, and that's why I think it's weird.

## How does git work?
So, what do I do first to get git going?
First thing I'm going is going to the [git docs](https://git-scm.com/docs). 
In the [git tutorial](https://git-scm.com/docs/gittutorial), there are simple instructions for placing a project under git revision control.
just cd into the project folder and
```
$ git init
```
Git will reply
```
Initialized empty Git repository in .git/
```
I have to use `ls -a` to see the git repository in the directory, so I guess it's typically hidden.

Next I am going to tell Git to take a snapshot of the contents of all files under the current directory with *git add*:
```
$ git add .
```
