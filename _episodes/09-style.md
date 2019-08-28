---
title: "Creating modules"
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

We are now going to add some code to our package. Delete what is in the `molecule` module and add the following.

~~~
"""
molecule.py
A python package for the MolSSI Software Summer School.

Contains a molecule class
"""

import numpy as np

from .measure import calculate_angle, calculate_distance


class Molecule:
    def __init__(self, name, symbols, coordinates):
        if isinstance(name, str):
            self.name = name
        else:
            raise TypeError("Name is not a string.")
        
        self.symbols = symbols
        self.coordinates = coordinates
        self.bonds = self.build_bond_list()
    
    @property
    def num_atoms(self):
        return len(self.coordinates)
    
    @property
    def coordinates(self):
        return self._coordinates
    
    @coordinates.setter
    def coordinates(self, new_coordinates):
        self._coordinates = new_coordinates
        self.bonds =self.build_bond_list()

    
    def build_bond_list(self, max_bond=2.93, min_bond=0):
        """Build a list of bonds based on a distance criteria."""
        num_atoms = len(coordinates)
        
        bonds = {}
        
        for atom1 in range(num_atoms):
            for atom2 in range(atom1, num_atoms):
                distance = calculate_distance(self.coordinates[atom1], self.coordinates[atom2])
                
                if distance > min_bond and distance < max_bond:
                    bonds[(atom1, atom2)] = distance
        
        return bonds
    

if __name__ == "__main__":
    # Do something if this file is invoked on its own
    pass
~~~
{: .language-python}

Make a second module called `measure.py` add the following.

~~~
"""
measure.py
This module contains functions to measure distance and angle between points.
"""

import numpy as np

def calculate_distance(rA, rB):
    """Calculate the distance between points A and B. Assumes rA and rB are numpy arrays."""
    dist_vec = (rA-rB)
    distance = np.linalg.norm(dist_vec)
    return distance

def calculate_angle(rA, rB, rC, degrees=False):
    """Calculate angle between points A, B, and C"""
    AB = rB - rA
    BC = rB - rC
    
    theta = np.arccos(np.dot(AB, BC) / (np.linalg.norm(AB)*np.linalg.norm(BC)))
    
    if degrees: 
        return np.degrees(theta)
    else:
        return theta
~~~
{: .language-python}

# Adding a function to our package
Let's try out our functions in the Python interpreter. Open a terminal and start Python.

Then

~~~
>>> import geometry_analysis
>>> import numpy as np
>>> r1 = np.array([0,0,0])
>>> r2 = np.array([0, 0.1, 0])
>>> geometry_analysis.calculate_distance(r1, r2)
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

We can create and edit our docstrings to be much more useful and informative, similar to the docstrings in numpy.

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

For our `distance` function, our `Returns` section looks like the following.

~~~
Returns
-------
distance: float
    The distance between two points.
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

Edit your code to have docstrings describing the functions.

~~~
"""
molecule.py
A python package for the MolSSI Software Summer School.

Contains a molecule class
"""

import numpy as np

from .measure import calculate_angle, calculate_distance


class Molecule:
    def __init__(self, name, symbols, coordinates):
        if isinstance(name, str):
            self.name = name
        else:
            raise TypeError("Name is not a string.")
        
        self.symbols = symbols
        self.coordinates = coordinates
        self.bonds = self.build_bond_list()
    
    @property
    def num_atoms(self):
        return len(self.coordinates)
    
    @property
    def coordinates(self):
        return self._coordinates
    
    @coordinates.setter
    def coordinates(self, new_coordinates):
        self._coordinates = new_coordinates
        self.bonds =self.build_bond_list()

    
    def build_bond_list(self, max_bond=2.93, min_bond=0):
        """
        Build a list of bonds based on a distance criteria.

        Atoms within a specified distance of one another will be considered bonded.

        Parameters
        ----------
        max_bond : float, optional

        min_bond : float, optional

        Returns
        -------
        bond_list : list
            List of bonded atoms. Returned as list of tuples where the values are the atom indices.
        """
        num_atoms = len(coordinates)
        
        bonds = {}
        
        for atom1 in range(num_atoms):
            for atom2 in range(atom1, num_atoms):
                distance = calculate_distance(self.coordinates[atom1], self.coordinates[atom2])
                
                if distance > min_bond and distance < max_bond:
                    bonds[(atom1, atom2)] = distance
        
        return bonds
    

if __name__ == "__main__":
    # Do something if this file is invoked on its own
    pass
~~~
{: .language-python}

Make a second module called `measure.py` add the following.

~~~
"""
measure.py
This module contains functions to measure distance and angle between points.
"""

import numpy as np

def calculate_distance(rA, rB):
    """Calculate the distance between points A and B.
    
    Parameters
    ----------
    rA : numpy array
        The x, y, z coordinates of point A
    rB : numpy array
        The x, y, z coordinates of point B
    
    Returns
    -------
    distance : float
        The distance between points A and B.
    
    Examples
    --------
    >>> calculate_distance(np.array([0, 0, 0], [0, 0.1, 0]))
    0.1
    """
    dist_vec = (rA-rB)
    distance = np.linalg.norm(dist_vec)
    return distance

def calculate_angle(rA, rB, rC, degrees=False):
    """Calculate angle between points A, B, and C
    
    Parameters
    ----------
    rA : numpy array
        The x, y, z coordinates of point A
    rB : numpy array
        The x, y, z coordinates of point B
    degrees : bool, optional
        Return the calculated angle in degrees.
    
    Returns
    -------
    angle : float
        The distance between points A and B.
    """
    AB = rB - rA
    BC = rB - rC
    
    theta = np.arccos(np.dot(AB, BC) / (np.linalg.norm(AB)*np.linalg.norm(BC)))
    
    if degrees: 
        return np.degrees(theta)
    else:
        return theta
~~~
{: .language-python}



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
$ yapf -i geometry.py
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

In your terminal, run yapf with the following command 

~~~
$ yapf FILE_NAME
~~~

This will output to your screen how yapf would format your file.

You can also see the diff by using the -d option

~~~
$ yapf -d FILE_NAME
~~~
{: .language-bash}


or, you can overwrite your version of the file. 

~~~
$ yapf -i molecule.py
~~~
{: .language-bash}

Here, the `-i` flag indicates to do this "in-place", this means your file will be overwritten with yapf's changes. If you examine the file after running yapf, you should see that it is returned to an easily readable format.

Test that your code works in the python prompt and commit these changes.
