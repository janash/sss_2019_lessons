---
title: "Using conda"
teaching: 15
exercises: 3
questions:
- "How can I use conda to create new environments?"
- "How can I use conda to install packages?"
- "Why should I use python environments during development?"
objectives:
- "Understand how to create and activate environments with conda"
keypoints:
- "conda is a package manager that can be used to create environments and install packages."
---

## Using Anaconda and conda

Use of [Anaconda] with its package manager, `conda`, greatly simplifies package installation and environment management.

`conda` is a general package manager, meaning that it can install dependencies and packages in languages besides Python, unlike `pip` (which is Python's package manager). Both `pip` and `conda` can be used to install packages.

### Python environments <a name="python_environment"></a>
A `conda` environment contains a specific collection of packages you have installed. This means that packages are isolated, and installed only for a specific environment -- you can have several environments each with different installed packages, or different versions of installed packages in different environments.

It's considered a best practice to create a new Python environment for each project you work on.

To create an environment for this project using `conda`,

~~~
$ conda create --name sss_2019 python=3.7
~~~
{: .language-bash}

For other projects, you should replace `molssi_devops` with a descriptive name for your project. `conda` also allows you to specify the python version to use with the environment. Here, `python=3.6` specifies that we want to use Python 3.6 in this environment. Executing this command will list the environment location and a list of Python packages to be installed. Choose `y(es)` when prompted.

Activate the environment using the command

~~~
$ conda activate sss_2019
~~~
{: .language-bash}

Once you've activated an environment, the name of the environment will be in parenthesis at the front of your command line prompt.

If you wanted to create an environment for testing your code in Python 2.7, for example, you could use the command

~~~
$ conda create --name sss_201927 python=2.7
~~~
{: .language-bash}

When this environment is activated, Python 2.7 will be used instead of Python 3.7.

To see a list of all your environments

~~~
$ conda info --envs
~~~
{: .language-bash}

To deactivate an environment, type

~~~
$ conda deactivate
~~~
{: .language-bash}

> ## Check your understanding
> How can you activate the environment `sss_2019`. How can you tell that you're using the environment?
>> ## Answer
>> You activate the environment using the command 
>> ~~~
>> $ conda activate sss_2019
>> ~~~
>> {: .language-bash}
>>
>> You can tell you are in the environment because it will appear at the beginning of your command prompt.
> {: .solution}
{: .challenge}

### Package installation using conda
Using `conda`, we can install packages to our environments. **Note**: Make sure you have activated the environment where you want to install packages.

To list all the Python packages installed in an environment, first activate it, then type

~~~
$ conda list
~~~
{: .language-bash}

Packages can be installed using the `conda install package_name` command. For example, to install NumPy,

~~~
$ conda install numpy
~~~
{: .language-bash}

Further, the desired version of NumPy can be specified:

~~~
$ conda install numpy=1.15
~~~
{: .language-bash}


[anaconda]: https://www.anaconda.com
[https://www.anaconda.com/download]: https://www.anaconda.com/download
[Download and install git for your operating system.]: https://git-scm.com/downloads
[python]: https://python.org
[github.com]: https://github.coms
[video-mac]: https://www.youtube.com/watch?v=TcSAln46u9U
[video-windows]: https://www.youtube.com/watch?v=xxQ0mzZ8UvA
[wsl-windows]: https://docs.microsoft.com/en-us/windows/wsl/install-win10
[CMS CookieCutter]: https://github.com/MolSSI/cookiecutter-cms