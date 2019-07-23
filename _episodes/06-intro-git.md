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
In this section, we are going to  create a git repository.

 
First, use a terminal to `cd` into the directory where you intend to work today.

I will make a folder on my Dekstop (`sss_2019_tutorials`), then make anothe directory (`day_2`) inside that directory. Then, `cd` into your newly created directories.

~~~
$ mkdir sss_2019_tutorials
$ cd sss_2019_tutorials
$ mkdir day_2
$ cd day_2
~~~
{: .language-bash}

Download or copy the script [here](https://gist.github.com/janash/f144d291a280cb50baa6028cdd035a70). This is a GitHub gist which has some geometry analysis functions that we wrote when working with QCArchive.

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
$ git log
~~~
{: .bash}

You will get an output resembling the following:
~~~
fatal: your current branch 'master' does not have any commits yet
~~~
{: .output}

When we have more commits (or versions) of our code, `git log` will show a history of these commits. Right now, we don't have any commits.

~~~
$ git status
~~~
{: .bash}

~~~
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	geometry.py
~~~
{: .output}

This tells us that we are on the `master` branch, and that we haven't made any commits yet. It shows the one file we have in the directory, `geometry.py`, but tells us that we are not tracking the file.

In order for `git` to keep track of a file's history, we have to tell it to. 

## The 3 steps of a commit

#### git add, git status, git commit

Making a commit is like making a checkpoint for a particular version of your code. You can easily return to, or revert to that checkpoint.

To create the checkpoint, we first have to modify the file. We might modify *many* files at a time in a repository. Thus, the first step in creating a checkpoint (or commit) is to tell `git` which files we want to include in the checkpoint. We do this with a command called `git add`. This adds files to what is called the *staging area*.

Let's look at our output from `git status` again.

~~~
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	geometry.py
~~~
{: .output}

Git even tells us to use `git add` to include what will be committed. Let's follow the instructions and tell `git` that we want to create a checkpoint with the current version of `geometry.py`.

~~~
$ git add geometry.py
~~~

~~~
$ git status
~~~
{: .language-bash}

~~~
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   geometry.py
~~~
{: .output}

We are now on the second step of creating a commit. We have `added` our files to the staging area. In our case, we only have one file in the staging area, but we could add more if we needed.

To create the checkpoint, or commit, we will now use the `git commit` command. We add a `-m` after the command for "message." Whenever you create a commit, you should write a message about what the commit does.

~~~
$ git commit -m "add geometry module"
~~~
{: .bash}

~~~
[master (root-commit) bd05b2a] add geometry module
 1 file changed, 36 insertions(+)
 create mode 100644 geometry.py
~~~
{: .output}

Now when we look at our log using `git log`, we see the commit we just made along with information about the author and the date of the commit.

Let's now create a README. This is a file which will describe what is in this directory. Open `README.md` in your text editor of choice. 

~~~
# 2019 MolSSI Software Summer School

This repository contains material for Day 2 of the 2019 MolSSI Software Summer School.
~~~

This file is using a language called [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet). 

> ## Check your understanding
> Create a commit for these changes to your repository.
>> ## Answer
>> To add the file to the staging area and tell `git` we would like to track the changes to the file, we first use the `git add` command.
>> ~~~
>> $ git add README.md
>> ~~~
>> {: .language-bash}
>> Now the file is *staged* for a commit.
>> Next, create the commit using the `git commit` command.
>> ~~~
>> $ git commit -m "add README with project description"
>> ~~~
>> {: .language-bash}
> {: .solution}
{: .challenge}

Once you have saved this file, type

~~~
$ git log
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
commit b551dfec58c4cae4e6790feb31e6c078a2d01f7f (HEAD -> master)
Author: Jessica Nash <janash@vt.edu>
Date:   Mon Jul 22 22:30:38 2019 -0500

    add README with project description

commit bd05b2a70ba694cab8836e750648f364c813049d
Author: Jessica Nash <janash@vt.edu>
Date:   Mon Jul 22 22:21:39 2019 -0500

    add geometry module
~~~
{: .output}

We now have a log with two commits. This means there are two versions of the repository we are working in.

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes the commit's full identifier,the commit's author, when it was created, and the commit title.

~~~
commit b551dfec58c4cae4e6790feb31e6c078a2d01f7f (HEAD -> master)
Author: Jessica Nash <janash@vt.edu>
Date:   Mon Jul 22 22:30:38 2019 -0500

    add README with project description

commit bd05b2a70ba694cab8836e750648f364c813049d
Author: Jessica Nash <janash@vt.edu>
Date:   Mon Jul 22 22:21:39 2019 -0500

    add geometry module
~~~


Let's make another edit to the readme. Open your readme file and add another line to describe the project.

~~~
# 2019 MolSSI Software Summer School

This repository contains material for Day 2 of the 2019 MolSSI Software Summer School.

We are learning about git.
~~~

Commit this change.

~~~
git add .
git commit -m "update readme to include information about git"
~~~

Now, when you check git status, you will see three commits.

We can see differences in files between commits using git diff.

~~~
$ git diff HEAD~1
~~~
{: .language-bash}

Here HEAD refers to the point in our commit history (and current branch). When we use `~1`, we are asking git to show us the different of the current point minus one commit.

~~~
diff --git a/README.md b/README.md
index 69af7e4..bf01502 100644
--- a/README.md
+++ b/README.md
@@ -1,3 +1,5 @@
 # 2019 MolSSI Software Summer School

 This repository contains material for Day 2 of the 2019 MolSSI Software Summer School.
+
+We are learning about git.
~~~
{: .output}

## Viewing out previous versions

If you need to check out a previous version

~~~
$ git checkout COMMIT_ID
~~~

This will temporarily revert the repository to whatever the state was at the specified commit ID.

Let's checkout the version before we made the most recent edit to the README.

~~~
$ git log --oneline
~~~

~~~
d14b67b (HEAD -> master) update readme to include information about git
b551dfe add README with project description
bd05b2a add geometry module
~~~
{: .output}

To revert to the version of the repository where we added the readme, use the git checkout command with the appropriate commit id.

~~~
$ git checkout b551dfe
~~~
{: .language-bash}

To return to the most recent point,

~~~
$ git checkout master
~~~

## Putting your repository on GitHub.
Now, let's put this project on GitHub so that we can share it with others. In your browser, navigate to `github.com`. Log in to you account if you are not already logged in. On the left side of the page, click the green button that says `New` to create a new repository. Give the repository the name `geometry_project`.

Since we are going to be pushing an existing repository, we will create a blank project.

Click `Create repository`.

Now, GitHub very helpfully gives us directions for how to get our code on GitHub.

Before we follow these directions, let's look at a few things in the repository. When you want to be able to put your code online in a repository, you have to add what git calls `remotes`. Currently, our repository has no remotes. See this by typing

~~~
$ git remote -v
~~~
{: .language-bash}

You should see no output. Now, follow the instructions on GitHub under "...or push an existing repository from the command line"
~~~
$ git remote add origin URL_OF_YOUR_REPOSITORY
$ git push -u origin master
~~~
{: .language-bash}

The first command adds a remote named `origin` and sets the URL to our repository. The second command pushes our repo to where we have set as origin. The word `master` means we are pushing the `master` branch.

Now if you refresh the GitHub webpage you should be able to see all of the new files you added to the repository.

## Ignoring files
## Ignoring Files

Sometimes while you work on a project, you may end up creating some temporary files.
For example, if your text editor is Emacs, you may end up with lots of files called `<filename>~`.
By default, Git tracks all files, including these.
This tends to be annoying, since it means that any time you do "git status", all of these unimportant files show up.

We are now going to find out how to tell Git to ignore these files, so that it doesn't keep telling us about them ever time we do "git status".
Even if you aren't working with Emacs, someone else working on your project might, so let's do the courtesy of telling Git not to track these temporary files.
First, lets ensure that we have a few dummy files:

~~~
$ touch testing.txt~
$ touch README.md~
$ ls -l
~~~

Now check what Git says about these files:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.md~
	testing.txt~

nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

You can tell github what to ignore by using the `.gitignore` file. Create a file called `.gitignore` and add the following

~~~
# emacs
*~
~~~

Now do "git status" again. Notice that the files we added are no longer recognized by git.

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   .gitignore

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

We want these additions to .gitignore to become a permanent part of the repository, so do

~~~
$ git add .gitignore
$ git commit -m "Ignores Emacs temporary files and data directory"
$ git push
~~~
{: .bash}

One nice feature of .gitignore is that prevents us from accidentally adding a file that shouldn't be part of the repository.
For example:

~~~
$ git add README.md~
~~~
{: .bash}

~~~
The following paths are ignored by one of your .gitignore files:
README.md~
Use -f if you really want to add them.
~~~
{: .output}

It is possible to override this with the "-f" option for git add.

When you initialize repository on GitHub, you can create a `.gitignore` of a specified language. You can also find a set of .gitignores from github [here](https://github.com/github/gitignore).

Here is the one for [Python](https://github.com/github/gitignore/blob/master/Python.gitignore).

{: .bash}



{% include links.md %}
[GitHub]: https://github.com
[GitLab]: https://gitlab.com/
[BitBucket]: https://bitbucket.org/
