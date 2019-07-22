---
title: "Intro to version control with git"
teaching: 20
exercises: 5
questions:
- "How do I use git and GitHub?"
objectives:
- "Explain the purpose of version control."
- "Introduce common git commands."
- "Explain use of .gitignore file"
- "Resolve a merge conflict"
keypoints:
- "Git provides a way to track changes and differences between versions."
- "git can be used with an online repository (GitHub) so that you can access a copy of your code from any machine"
---

## Version Control

Version control keeps a complete history of your work on a given project. It
facilitates collaboration on projects where everyone can work freely on a part
of the project without overriding others’ changes. You can move between past
versions and rollback when needed. Also, you can review the
history of your project through commit messages that describe changes on the source code
and see what exactly has been modified in any given commit. You can see who made the
changes and when it happened.

This is greatly beneficial whether you are working independently or within a
team.

> ## git vs. GitHub
>
> `git` is the software used for version control, while GitHub is a hosting service. You can use `git` locally (without using an online hosting service), or you can use it with other hosting services such as GitLab or BitBucket.
> Other examples of version control software include SVN, and Mercurial.
>
{: .callout}

MolSSI recommends using the software `git` for version control, and [GitHub] as a hosting service, though there are other options.

Recommended Hosting Service: [GitHub]  
Other hosting Services: [GitLab], [BitBucket]

## Making Commits

Now that we have Git configured, let's try using Git to do something that is actually helpful.
In this section, we are going to  create our git repository.


First, use a terminal to `cd` into the directory where you intend to work today.

I will make a folder on my Dekstop (`sss_2019_tutorials`), then make anothe directory (`day_1`) inside that directory. Then, `cd` into your newly created directories.

~~~
$ mkdir sss_2019_tutorials
$ cd sss_2019_tutorials
$ mkdir day_1
$ cd day_1
~~~
{: .language-bash}

To initialize a repository, the command you use is 

~~~
$ git init
~~~
{: .language-bash}

This tell the git software that we want to start tracking files in this folder and keeping a history of our work.

## How to tell if you are working in a git repository

~~~
$ ls -la
~~~
{: .bash}

Here, the `-la` says that we want to list the files in long format (`-l`), and show hidden files (`-a`). You should see several files starting with `.git`. In particular, `.git` is a directory where `git` stores the repository data. We can tell from this output that we are in a git repository.

Next, type

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
nothing to commit, working tree clean
~~~
{: .output}

This tells us that we are on the `master` branch, and that no files have been changed since the last commit.

Next, type
~~~
$ git log
~~~
{: .bash}

You will get an output resembling the following:
~~~
fatal: your current branch 'master' does not have any commits yet
~~~
{: .output}

When we have more commits (or versions) of our code, `git log` will show a history of these commits. Right now, we have only one commit - the one created by the CMS CookieCutter.

Now, we will change some files and use `git` to track those changes. Let's create our README. This is a file which will describe what is in this directory. Open `README.md` in your text editor of choice. 

~~~
# 2019 MolSSI Software Summer School

This repository contains material for Day 1 of the 2019 MolSSI Software Summer School.
~~~

This file is using a language called [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet). 

Once you have saved this file, type

~~~
$ git status
~~~
{: .language-bash}

and you should see

~~~
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

This tells that since the last commit, we have modified the file `README.md`. In order to commit these changes using `git`, we first have to "stage" the changes using `git add`, then store the changes using `git commit`.

~~~
$ git add README.md
~~~
{: .language-bash}

Now, when we check the status, we should see the following:

~~~
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.md
~~~
{: .output}

Now, we see that we have staged this file to be committed. Commit the file

~~~
$ git commit -m "Add readme with description."
~~~
{: .language-bash}

Now, check `git status` and `git log`. You should see the following:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
nothing to commit, working tree clean
~~~
{: .output}

~~~
$ git log
~~~
{: .bash}

~~~
commit 2ac484309fe62f9a847a4daae1a1fa078dca306b (HEAD -> master)
Author: Jessica Nash <janash@vt.edu>
Date:   Thu July 18 12:47:45 2019 -0500

    Add readme with description.
~~~
{: .output}

So far, we only have one commit. Let's edit our README a little more. Open the file again, and add your name.

> # Check your understanding
> How do you make a record of your change?
>> ## ANSWER
>> git status, git add, git commit 
> {: .solution}
{: .challenge}

~~~
$ git status
~~~
{: .language-bash}

Now, we have a second commit at the top of our git log with the message we just typed. `git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes the commit's full identifier,the commit's author, when it was created, and the commit title.

We can see differences in files between commits using git diff

~~~
$ git diff HEAD~1
~~~
{: .language-bash}

~~~
$ git checkout COMMIT_HASH FILE_NAME
~~~

~~~
$ git checkout master
~~~

## Checking out a previous version


{% include links.md %}
[GitHub]: https://github.com
[GitLab]: https://gitlab.com/
[BitBucket]: https://bitbucket.org/
