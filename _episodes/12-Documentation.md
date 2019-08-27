---
title: "Documentation"
teaching: 30
exercises: 40
questions:
- "How can we document out module?"
objectives:
- "Run the lesson checking script and interpret its output correctly."
- "Explain in-code documentation"
- "Explain documentation tools like Read The Docs"
keypoints:
- "Some documentation is better than no documentation"
---

This episode discusses documentation strategies.

Documentation must be provided to allow for use, development, and maintenance
of the code. Documentation is often overlooked by developers since it is
tedious and boring, however good documentation is an extremely good habit to
develop.

The documentation typically involves several components:

 - Build requirements and dependencies (if applicable)
 - How to compile/build/test/install
 - How to use the software (through the API or through inputs)
 - Some examples
 
## Developer Documentation
Another aspect of documentation is code documentation. This is very
important for further development and maintenance, including by yourself in the
future. This typically includes documenting various internal files, functions,
and classes; what pieces of code do; and most importantly, the reasoning behind
some decisions as to why some code was written a particular way.

Documentation should be kept up to date with changes in the code, which is not
an easy task for large, fast-moving codebases. However, slightly out-of-date
documentation is generally preferable to no documentation.

It is recommended that the examples provided within the documentation are
compiled and/or run regularly (if possible, as part of the testing of the
software) to ensure that it does not become neglected and out-of-date,
confusing users.

## In-code documentation

## README documentation

- Ok to document in README for simple modules

## Sphinx for complex modules

When you want to improve your documentation strategies for Python packages, use [Sphinx](https://www.sphinx-doc.org/en/master/). Sphinx is a tool for creating documentation, and was originally created for documentation of the Python programming language. Many projects you are familiary with use Sphinx for documentation - including numpy, matplotlib, and pytest. To see a list of project that use Sphinx, click [here](https://www.sphinx-doc.org/en/master/examples.html).

CookieCutter has already set up files which we need to get started with Sphinx.

## Using Sphinx to build documentation

To use Sphinx to document your modules, you will first need to install Sphinx. This command installs Sphinx, and the Sphinx Read The Docs theme.

~~~
$ conda install sphinx sphinx_rtd_theme 
~~~
{: .language-bash}

The files which CookieCutter has set up for Sphinx are in the `docs` folder. Navigate to that directory.

~~~
$ cd docs
~~~
{: .langauge-bash}

Next, look at the files in the directory

~~~
$ ls
~~~
{: .bash}

~~~
Makefile   README.md  _static    _templates conf.py    index.rst  make.bat
~~~
{: .output}

Sphinx builds documenation from reStructred text files. We have one restructured text file here (`index.rst`). When you build your documentation, this will be the index, or main page of your documentation.

To build the default copy of your documentation, type

~~~
$ make html
~~~
{: .language-bash}

This command tells Sphinx to generate your documentation. With this command, we are building HTML files from the reStructured text file which can be put online. The HTML files Sphinx has built are in the `_build` directory (this is set in your `conf.py` file.)

Look at files in `_build/html`. You can open these files in your browser.

These html pages are built from rst files. 

Add a description to your `index.rst`. 

~~~
This module has a Molecule object and provides some manipulation functions.
~~~

Next, clean your previous build and rebuild your pages.

~~~
$ make clean
$ make html
~~~
{: .language-bash}

Refresh the page in your browser to see the page with the description.

You can also add additional pages. For example, you might want to include a 'Quickstart Guide' to running your program. Create this file and fill in some contents.

~~~
touch quickstart.rst
~~~
{: .language-bash}


Add some content to quickstart.rst

~~~
Quickstart Guide
=============================

To do a developmental install, type

`pip install -e .`

Dependencies
**************
You need to install `numpy`

Subheader
-------------------
Sample subheader
~~~

To get this page to be visible from your index file, we will need to add it to the table of contents (TOC). There is a section in your `index.rst` called `toctree`. We will add this new page below.

~~~
.. toctree::
   :maxdepth: 2
   :caption: Contents:

   quickstart
~~~

Now, when you generate documentation, there should be a link to this page. Note that you add the __file name__ in the TOC Tree, but the title of the page ('Quickstart Guide' is what shows up in the TOC tree)

To generate your documentation with the modules documented from your docstrings, we will use Sphinx autodoc. Open your `conf.py` file and find the `extensions` section.

Add the following

~~~
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.autosummary',
    'sphinx.ext.mathjax',
    'sphinx.ext.napoleon',
]

autosummary_generate = True
~~~
{: .language-python}

We've added a few extensions here which will allow us to pull doc strings from our Python modules (`sphinx.ext.autosummary`, `sphinx.ext.autodoc`), and another which we use because our docstrings are `NumPy` style (`sphinx.ext.napoleon`). Next, we have added the line `autosummary_generate = True` to allow us to pull auto summaries from our modules and functions.

Next, we add a page for documenting our package API which will use `autodoc` and `autosummary` from Sphinx.

~~~
$ touch api.rst
~~~
{: .language-bash}

In the api.rst, add documentation for the `calculate_distance` function.

~~~
API Documentation
=================

Data for distances

.. autosummary::
    :toctree: autosummary

    geometry_analysis.calculate_distance
~~~

Next, add the `api` page to your TOCTree

~~~
.. geometry_analysis documentation master file, created by
   sphinx-quickstart on Thu Mar 15 13:55:56 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Overview
=========================================================

This module has a Molecule object and provides some manipulation.

Available techniques:

 - distance
 - *bonds*

A link to `Google <www.google.com>`_.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   quickstart
   api



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
~~~

Next, rebuild your documentation. In the `docs` folder

~~~
$ make clean
$ make html
~~~
{: .language-bash}

Now, you should see the doc string for the `calculate_distance` function pulled out as documentation.

> ## Exercise
> Add documentation for the `calculate_angle` function.
>
>> ## Answer 
>> ~~~
>> .. autosummary::
>>     :toctree: autosummary
>> 
>>     geometry_analysis.calculate_angle
>> ~~~
> {: .solution}
{: .challenge}

## Hosting your documentation

### Read The Docs
We recommend hosing your  documentation on [Read The Docs](https://readthedocs.org/). With this service, you can enable the building of your documentation every time you push to your repository.

Go to the [Read the Docs](https://readthedocs.org/) website. Log in with your GitHub username and hook the repository to read the docs. Push to the repository, or trigger a build on the site. You should see you documentation. However, you will not see your docstring information.

#### Using autodoc and autosummary on RTD
If you would like to use `autodoc` on Read the Docs, you will need a configuration file and an environment file specifically for documentation. In order for the RTD site to build your auto documentation, it has to build and import your module. Since we have dependencies, we will need provide an environment file and a configuration file.

Copy the environment file we use for testing. From your `docs` folder, type

~~~
$ cp ../devtools/conda-envs/test_env.yaml doc_env.yaml
~~~
{: .language-bash}

Edit the `doc_env.yaml` file to only have the packages we need for installing and using the package.

Contents of `doc_env.yaml`

~~~
name: documentation
channels:
dependencies:
    # Base depends
  - python
  - pip

  # Dependencies
  - numpy

    # Pip-only installs
  #- pip:
  #  - codecov
~~~

Next, we will create the configuration file for Read the Docs.

~~~
cd ../
touch .readthedocs.yml
~~~

Add to .readthedocs.yml

~~~
# .readthedocs.yml

version: 2

build:
  image: latest

python:
  version: 3.6
  install:
    - method: pip
      path: .

conda:
  environment: docs/doc_env.yaml
~~~

Commit to these changes and push to your repository. You should now see your documentation strings.

[Example repo](https://molssi-sss-2019-ga-example.readthedocs.io/en/latest/)



{% include links.md %}
