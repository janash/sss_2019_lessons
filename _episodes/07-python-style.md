---
title: "Python Coding Style"
teaching: 30
exercises: 15
questions:
- "How can I write python code that is readable?"
objectives:
- "Understand how to follow PEP8 style for Python."
- "Understand what docstrings are and their importance."
- "Learn to write docstrings in numpy style"
keypoints:
- "Your code should adhere to standards outlined in PEP8 so that it easily readable by others."
- "All functions and modules should be documented with docstrings."
---

# Adding a function to our package
Let's try out our functions in the Python interpreter. Open a terminal and start Python.

Then

~~~
>>> import geometry
>>> import numpy as np
>>> r1 = np.array([0,0,0])
>>> r2 = np.array([0, 0.1, 0])
>>> geometry.calculate_distance(r1, r2)
0.1
~~~
{: .language-python}

Your function should work. Now let's look at what happens if we call 'help' on this function

~~~
>>> help(geometry.calculate_distance)
~~~

~~~
Help on function calculate_distance in module geometry:

calculate_distance(rA, rB)
    Calculate the distance between points A and B. Assumes rA and rB are numpy arrays.
~~~

The code above calls Python's built-in function, `help`. For our function, it displays the  comment (surrounded in triple quotes) (called a `docstring`), that is written beneath the function definition.

This docstring looks a lot different than those we've seen for numpy. For example, consider the `np.mean` function.

~~~
>>> help(np.mean)
~~~
{: .language-python}

~~~
Help on function mean in module numpy.core.fromnumeric:

mean(a, axis=None, dtype=None, out=None, keepdims=<no value>)
    Compute the arithmetic mean along the specified axis.

    Returns the average of the array elements.  The average is taken over
    the flattened array by default, otherwise over the specified axis.
    `float64` intermediate and return values are used for integer inputs.

    Parameters
    ----------
    a : array_like
        Array containing numbers whose mean is desired. If `a` is not an
        array, a conversion is attempted.
    axis : None or int or tuple of ints, optional
        Axis or axes along which the means are computed. The default is to
        compute the mean of the flattened array.

        .. versionadded:: 1.7.0

        If this is a tuple of ints, a mean is performed over multiple axes,
        instead of a single axis or all the axes as before.
    dtype : data-type, optional
~~~
{: .output}

We can edit our docstrings to be much more useful and informative, similar to the docstrings in numpy.


## Docstrings
We've will now add a multi-line comment (called a `docstring`, short for "doumentation string"), to the beginning of our function. Docstrings **are the first statment after a function or module definition** and are opened and closed with three quotes.

The docstring should explain what the function or module does (and not how it is done).

> ## The `__doc__` attribute
>
> When you add a docstring to a function or module, python automatically adds this to the `__doc__` attribute of the object.
>
> You can also see an object's docstring by typing `object.__doc__` into the Python interpreter. For example, to see the docstring associated with the canvas function, `molssi_devops.canvas.__doc__` into the Python interpreter (after importing `molssi_devops`, of course.)
{: .callout}

### Sections of a Docstring
There are many ways you could format this docstring (different styles/conventions). We recommend using [numpy style docstrings], and this is what the example above and `mean` funtion are written in.

Each docstring has a number of sections which are separated by headings. Headings should be underlined with hyphens (`-----`). There are many options for sections, we will only cover the most relevant here. If you would like to see a full list, check out the documentation for [numpy style docstrings].

#### 1. Short summary
A one-line summary that does not use the variable name or the function name. In our `mean` function, this corresponds to the following.


#### 2. Extended summary
A few sentences giving a detailed description of the function or module. This section should be used to clarify *functionality*, not to discuss implementation.  

We do not have an extended summary in our `mean` function, since it is relatively straightforward.

#### 3. Parameters
This section contains a description of the function arguments - keywords and expected types.

The parameters for our `mean` function is shown below:

Here, you can see that the parameter section begins with the section title ("Parameters"), followed by a line of hypens ("----"). On the next line, we have the argument name(s), then a colon followed by the input type of the argument. The next line gives a more detailed description of the variable.

#### 4. Returns
This section is very similar to the `Parameters` section above. In contrast to the `Parameters` section, each returned value does not have to be named, but the type of the return value is required.

For our `mean` function, our `Returns` section looks like the following.

~~~
Returns
-------
mean_list: float
    The mean of the list
~~~
{: .language-python}

#### 5. Examples
This is an optional section to show examples of functionality. This section is meant to illustrate usage. Though this section is optional, its use is strongly encouraged.

Now that we've written a function in our project, we should commit our changes and push to [GitHub].

~~~
$ git add .
$ git commit -m "add docstring to calculate bond function"
$ git push origin master
~~~
{: .bash}

## Coding Style

As a developer, you spend a lot of time thinking about writing your code. However, code is read much more often than it is written. Following a style guide will help others (and perhaps you in the future!) to read your code.

 For Python, the common convention for code style is called [PEP8]. PEP8 is a document that gives guidelines for best practices in Python coding style. PEP8 is a recommendation, not rule. However, you should follow this convention when possible.

 > ## Python PEP
 >
 > If you spend a lot of time programming in Python, you will see references to PEPs a lot. PEP stands for "Python Enhancement Proposal". These are design documents which provide information about features. PEPs come from the Python community, meaning anyone can author a PEP (however, there is a strict review process). PEPs are classified into three categories - standards, informational, or process.
 >
 > You can read more about PEPs in [Python's documentation](https://www.python.org/dev/peps/pep-0001/). PEP1 outlines what a PEP is and how they work.
 {: .callout}

If you look at PEP8, you will see that it is quite long. While you should definitely read it if you spend a lot of time programming in Python, there are luckily tools which will help us make sure our code is following PEP8 convention. We will use [yapf], an open source formatter for Python files from Google.

Install yapf using pip. In your terminal, type

~~~
$ pip install yapf
~~~
{: .language-bash}

In your terminal, run yapf with the following command -

~~~
$ yapf -i molssi_devops/molssi_math.py
~~~
{: .language-bash}

Here, the `-i` flag indicates to do this "in-place", this means your file will be overwritten with yapf's changes. If you examine the file after running yapf, you should see that it is returned to an easily readable format.

Commit these Changes

Now that we've written some docstrings in our project, we should commit our changes and push to [GitHub].

~~~
$ git add .
$ git commit -m "run yapf on molssi_math"
$ git push origin master
~~~
{: .bash}

[PEP8]: https://www.python.org/dev/peps/pep-0008/
[YAPF]: https://github.com/google/yapf
[numpy style docstrings]: https://docs.scipy.org/doc/numpy/docs/howto_document.html#numpydoc-docstring-guide
