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
 - Another aspect of documentation is code documentation. This is very
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

## RTD and Sphinx for complex modules

When you want to improve your documentation strategies use Sphinx and readthedocs.

~~~
$ conda install sphinx sphinx_rtd_theme 
~~~
{: .language-bash}

~~~
$ cd docs
~~~
{: .langauge-bash}

~~~
$ make html
~~~
{: .language-bash}

Look at files in `_build/html`.

These html pages are built from rst files. Look at and edit `index.rst`. Add a description at the top, save, and view the html.

~~~
This module contains a molecule class. You initialize the molecule with
names and coordinates, and a bond list is built based on distance
between atoms.
~~~

~~~
mkdir usage
cd usage
touch quickstart.rst
~~~
{: .language-bash}

Contents of quickstart.rst
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

Add this page to `index.rst`.

~~~

~~~

To generate the module index with documentation.

~~~
sphinx-apidoc -o source/ ../<package>
~~~

Next,

~~~
$ make html
~~~

Look at your html pages. We see that tests have been documented. We probably do not want this. The command for apidoc is

~~~
sphinx-apidoc [OPTIONS] -o <OUTPUT_PATH> <MODULE_PATH> [EXCLUDE_PATTERN, …]
~~~

We can exclude tests by

~~~
$ sphinx-apidoc -o source/ ../geometry_analysis ../geometry_analysis/*test*
~~~
{: .language-bash}

Our autodocumentation still doesn't look the way we want. We are using numpy style docstrings, but Sphinx usually works with restructured text. Fix this by adding the following to your `conf.py` file.

~~~
extensions = ['sphinx.ext.napoleon']
~~~

Remove the older files and rebuild.

## Putting on Read The Docs

To put our auto generated documentation on Read The Docs, we will need a configuration file. In order for the RTD site to build your auto documentation, it has to build and import your module. Since we have dependencies, we will need to provide an environment file and a configuration file.

Copy the environment file we use for testing.

~~~
$ cp ../devtools/conda-envs/test_env.yaml doc_env.yaml
~~~
{: .language-bash}

Remove QCArchive because we only use it for testing.

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
  environment: doc_env.yaml
~~~




{% include links.md %}
