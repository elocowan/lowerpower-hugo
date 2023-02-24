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
Now there is a whole bunch of stuff in the .git folder.
```
Ethans-MacBook-Air:.git dreamer$ ls
HEAD		description	index		objects
config		hooks		info		refs
```
Then the commit:
```
$ git commit
```
This command prompts me for a commit message.
It happens to launch VSCode.
What I want is for it to open a new terminal window and prompt me to enter the commit message in Vim.
The answer is in the docs about [setup and config](https://git-scm.com/book/en/v2/Appendix-C:-Git-Commands-Setup-and-Config).
```
git config --global core.editor "vim --nofork"
```
Next time it should open the prompt to write a commit message in Vim in the terminal.

Mission accomplished.
Now there is a snapshot of the whole project as it stands now, at the end of the third day.
I have gotten all of the posts into the website.
The next step is to go in and edit them plus format them.

## Next time
Some open questions are:
- How should I organize the posts? Because it's nice to have them in chronological order, but it would also be nice to set things up in series.
- What are some examples of other folks' documentation websites?
- How can I set up next and previous links through a series of posts
- It would be cool to make a Dream series, a Feldenkrais series, and a Computer series
    - is the best way to do this with Tags? Categories? something else?
