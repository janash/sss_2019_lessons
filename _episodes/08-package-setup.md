---
title: "Python Package Set-up and Structure"
teaching: 25
exercises: 0
questions:
- "What is the layout of a Python package?"
- "How can I quickly create the structure of a Python package?"
- "What license should I choose for my project?"
objectives:
- "Explain Python package structure."
- "Use the CMS CookieCutter to build a Python package."
keypoints:
- "There is a common way to structure Python packages"
- "You can use the CMS CookieCutter to quickly create the layout for a Python package"
---

## Examples of Python package structure
If you look at the GitHub repositories for several large Python packages such as [numpy], [scipy], or [scikit-learn], you will notice a lot of similarities between the directory layouts of these projects.

Having a similar, or standard way to lay out Python packages allows people to more easily understand and contribute to your code. We could make the structure by hand, however, it is much easier to use a tool to do this.

## Creating a Python package using CookieCutter
To create a skeletal structure for our project, we will use the MolSSI [CMS CookieCutter]. This will not only create our directory layout, but will also set up many tools we will use including testing, continuous integration, documentation, and git.

### Running CookieCutter
Once you have CookieCutter installed, you can run it. CookieCutter is a general tool for making copies of repositories. We will use it to make a copy of the MolSSI Python project template.

To run the [CMS CookieCutter], navigate to the directory where you would like to start your project, and type:

~~~
$ cookiecutter gh:molssi/cookiecutter-cms
~~~
{: .language-bash}

This command will bring up an interactive prompt which asks questions about your project. Here, the prompt is given first, followed by the default value in square brackets. Answer the questions according to the following.
If nothing is given after the colon (`:`), hit enter to use the default value.
~~~
project_name [ProjectName]: geometry_analysis
repo_name [geometry_analysis]:
first_module_name [geometry_analysis]: molecule
author_name [Your name (or your organization/company/team)]: *YOUR_NAME_HERE*
author_email [Your email (or your organization/company/team)]: *YOUR_EMAIL_ADDRESS_HERE*
description [A short description of the project.]: A python package for the MolSSI Software Summer School.

Select open_source_license:
1 - MIT
2 - BSD-3-Clause
3 - LGPLv3
4 - Not Open Source
Choose from 1, 2, 3, 4 (1, 2, 3, 4) [1]: 2

Select dependency_source:
1 - Prefer conda-forge over the default anaconda channel with pip fallback
2 - Prefer default anaconda channel with pip fallback
3 - Dependencies from pip only (no conda)

Choose from 1, 2, 3 (1, 2, 3) [1]:

Select Include_Windows_continuous_integration:
1 - y
2 - n
Choose from 1, 2 (1, 2) [1]:
~~~

### About these decisions

#### License Choice
Choosing which license to use is often confusing for new developers. Here, we have chosen the `BSD-3-Clause`. The `BSD-3-Clause` license is an open-source, permissive license (meaning that few requirements are placed on deveopers of derivative works), similar to the MIT license. However, it prohibits others from using the name of the project or its contributors to promote derived products without written consent.


> ## Types of Open-Source Licenses
>
> Open-source licenses can either be 'permissive' or 'copy-left'. Copy-left licenses require that derivative works also be open source. Out of the choices given above, MIT and BSD-3-Clause are permissive, while LGLLv3 is a copy left license.
>
> We recommend that you spend some time reading about licensing. One place to start is this [helpful guide] from the Chodera Lab.  
{: .callout}

#### Dependency Source
This section will decide what dependency source you are using for automated testing of your package on Travis (covered later).

Choosing a dependency source selects _where_ to get dependencies from, and also _what manager_ to use.
• The default: Pull packages from Conda-Forge, uses Conda to manage dependencies
• The second option: Pull packages from Conda’s Defaults channel, uses Conda to manage dependencies
• The third option: Pull packages from PyPi, uses pip to manage dependencies

It is highly recommended to deploy your package to the same location as your choice of dependencies since your tests should already be validated against it, and you will know all your package dependencies are in the same place. You do not have to nor are you limited to only deploy there, but you should test your package with every dependency source you plan to deploy to.

### Reviewing directory contents
Now we can examine the project layout the CookieCutter has set up for us. Navigate to the newly created `geometry_analysis` directory. You should see the following directory structure.

```
.
├── LICENSE                         <- License file
├── README.md                       <- Description of project which GitHub will render
├── appveyor.yml                    <- AppVeyor config file for Windows testing (if chosen)
├── geometry_analysis
│   ├── __init__.py                 <- Basic Python Package import file
│   ├── geometry_analysis.py        <- Starting package module
│   ├── data                        <- Sample additional data (non-code) which can be packaged
│   │   ├── README.md
│   │   └── look_and_say.dat
│   ├── tests                       <- Unit test directory with sample tests
│   │   ├── __init__.py
│   │   └── test_molssi_math.py
│   └── _version.py                 <- Automatic version control with Versioneer
├── devtools                        <- Deployment, packaging, and CI helpers directory
│   ├── README.md
│   ├── conda-envs                  <- Environments for testing
│   │   └── test_env.yaml
│   ├── conda-recipe                <- Conda build and deployment skeleton
│   │   ├── bld.bat                 <- Win specific file, not present if Win CI not chosen
│   │   ├── build.sh
│   │   └── meta.yaml
│   ├── scripts
│   │   └── create_conda_env.py     <- OS agnostic Helper script to make conda environments based on simple flags
│   └── travis-ci
│       └── install.sh
├── docs                            <- Documentation template folder with many settings already filled in
│   ├── Makefile
│   ├── README.md                   <- Instructions on how to build the docs
│   ├── _static
│   ├── _templates
│   ├── conf.py
│   ├── index.rst
│   └── make.bat
├── setup.cfg                       <- Near-master config file to make house INI-like settings for Coverage, Flake8, YAPF, etc.
├── setup.py                        <- Your package's setup file for installing with additional options that can be set
├── versioneer.py                   <- Automatic version control with Versioneer
├── .github                         <- GitHub hooks for user contribution and pull request guides
│   ├── CONTRIBUTING.md
│   └── PULL_REQUEST_TEMPLATE.md
├── .codecov.yml                    <- Codecov config to help reduce its verbosity to more reasonable levels
├── .gitignore                      <- Stock helper file telling git what file name patterns to ignore when adding
└── .travis.yml                     <- Travis-CI config file for Linux and OSX testing
```
{: .output}

To visualize your project like above you will use "tree". If you do not have tree you can get using `sudo apt-get install tree` on linux, or `brew install tree` on Mac.

CookieCutter has created a lot of files! We will be working in the `geometry_analysis` folder initially to develop our functions and tests. The other created directories, `devtools`, and `docs`, are related to package deployment and documentation respectively.

> ## Packages and modules
>
> What 'packages' or 'modules' are in Python may be confusing.
> In general, 'module' refers to a single `.py` file containing Python definitions and statements. It may be imported for use in another module or script. The module name is determined by the file name. A function defined in a module is used (once the module is imported) using the syntax `module_name.function_name()`.
> 'Package' refers to a collection of Python modules.
>
> To read more about Python packages vs. modules, check out [Python's documentation].
{: .callout}

### Our first function
Once inside of the `geometry_analysis` folder (`geometry_analysis/geometry_analysis`), examine the files that are there. View the first module (`geometry_analysis.py`) in a text editor. We see a few things about this file. The top begins with a description of this module. Right now, that is the file name, followed by our short description, then the sentence "Handles the primary functions". We will change this to be more descriptive later. CookieCutter has also created a placeholder function in called `canvas`.  At the start of the `canvas` function, we have a `docstring`, which describes the function. Lastly, there is a `__main__` section in this file that defines what this file will do if run on it's own.

~~~
python geometry_analysis.py
~~~
{: .language-bash}

~~~
The code is but a canvas to our imagination.
	- Adapted from Henry David Thoreau
~~~
{: output}

CookieCutter has also added an `__init__.py` file. This file tells Python that the directories contain packages, and imports our `geometry_analysis` module.

### Reviewing `setup.py`
Return to the top directory (`geometry_analysis`). One of the files CookieCutter generated is a `setup.py` file. `setup.py` is the build script for [setuptools]. It tells setuptools about your package (such as the name and version) as well as which code files to include. We'll be using this file in the next section.

### Python local installs

For development work it is often recommended to do a "local" or "developer" install. This will allow you to import your package and use it from anywhere on your computer. A local install uses the `setup.py` file to install your package by inserting a link to your new project into your Python site-packages folder. To find the location of your site packages folder, you can check your
Python path. Open Python in the REPL (type `python` into your terminal window), and type

~~~
import sys
sys.path
~~~
{: .language-python}

This will give a list of locations python looks for packages when you do an import. One of the locations should end with `python3.7/site_packages`

To do a local install, type

~~~
$ pip install -e .
~~~
{: .language-bash}

Here, the `-e` indicates that we are installing this project in 'editable' mode (i.e. setuptools "develop mode"), while `.` indicates to install from the local directory (you could also specify a path here). Now, if you navigate to your site packages folder, you should see a link to `geometry_analysis` (`geometry-analysis.egg-link`). The folder has also been added to your path (check `sys.path` again.)

Now, we can use our package from any directory, similar to how we can use other installed packages like `numpy`. Open Python, and type

~~~
import geometry_analysis as gm
ga.molecule.canvas()
~~~
{: .language-python}

This should print a quote, as we saw when we looked at the `canvas` function above.

> ## Exercise
> What happens if we use `conda deactivate` and attempt to execute the code above? What if we switch directories?
>> ## Solution
>> If you are in the project directory, the code will still work. However, it will not work in any other location.
> {: .solution}
{: .challenge}


Optional dependencies can be installed as well with `pip install -e .[docs,tests]`


{% include links.md %}
[Python's documentation]: https://docs.python.org/3/tutorial/modules.html
[setup]: https://molssi-education.github.io/CMS-Python-DevOps/setup.html
[numpy]: https://github.com/numpy/numpy
[scipy]: https://github.com/scipy/scipy
[scikit-learn]: https://github.com/scikit-learn/scikit-learn
[CMS CookieCutter]: https://github.com/MolSSI/cookiecutter-cms
[Install cookiecutter]: https://cookiecutter.readthedocs.io/en/latest/installation.html
[setuptools]: https://packaging.python.org/key_projects/#setuptools
[helpful guide]: https://github.com/choderalab/software-development/blob/master/LICENSING_GUIDELINES.md
