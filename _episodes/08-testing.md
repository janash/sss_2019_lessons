---
title: "Python Testing"
teaching: 45
exercises: 10
questions:
- "How is a Python module tested?"
objectives:
- "Explain the overall structure of testing."
- "Explain the reasons why testing is important."
- "Understand how to write tests using the pytest framework."
keypoints:
- "A good set of tests covers individual functions/features __and__ behavior of the software as a whole."
- "It's better to write tests during development so you can check your progress along the way. The longer you wait to start the harder it is."
- "Try to test as much of your package as you can, but don't go overboard, most packages don't have 100% test coverage."
---

Until now, we have been writing functions and checking their behavior using an interactive Python interpreter and manually inspecting the output. While this seems to work, it can be tedious, and prone to error. In this lesson, we'll discuss how to write tests and run them using the `pytest` testing framework.

This episode explains the importance of code testing and demonstrates the possible capabilities.

## Why testing

Software should be tested regularly throughout the development cycle to ensure correct operation. Thorough testing is typically an afterthought, but for larger projects, it is essential for ensuring changes in some parts of the code do not negatively affect other parts.

**Software testing is checking the behavior of part of the code (such as a method, class, or a module) by comparing its expected output or behavior with the observed one. We will explain this in more details shortly.**


## Unit vs Regression vs Integration testing

There are many types of testing. There are three main levels of testing:

- **Unit tests**: the purpose is to verify that each part of the code is functioning as expected.
Unit testing is done on smaller units (such as single functions or classes) as you work on
your code.
This is helpful for catching errors in uncommonly-used parts of the code. Unit tests can
be added as new features are added, resulting in better code coverage.
In unit tests, you are testing a part of your code independent of any other factors;
therefore, you should avoid using the file system, databases, network, or any other
resources unless you are testing a function directly related to that resource.

- **Integration tests**: this is a more holistic approach where you test the interface
between modules, and how they combine and integrate together.

- **System tests**: where you test your system as a whole to check if meets all the
requirements.


Another important type of testing is **Regression tests**. In Regression tests,
given a known input, does the software correctly and consistently return the correct
values? This kind of testing can catch problems in previously working code that may
 has been broken by new changes or new features.

It is highly encouraged to have Unit tests that *cover* most of your code. It is
also helpful to have some Integration and System tests.

In this lesson, we are focusing on unit testing, along with regression testing.
Same concepts here can be applied to perform Integration tests across modules.
We will be using Python version 3.5 or above.

## The pytest testing framework

The Python testing framework was chosen to be [pytest](https://pytest.org) for this project.
Other testing frameworks are available (such as unittest and nose tests);
however, the authors believe the combination of easy [parametrization of tests](https://docs.pytest.org/en/latest/parametrize.html),
[fixtures](https://docs.pytest.org/en/latest/fixture.html), and [test marking](https://docs.pytest.org/en/latest/example/markers.html)
make `pytest` particularly suited for computational chemistry.

If you don't have `pytest` installed or it's not updated to version 3, install it using:
~~~
$ pip install -U pytest-cov
~~~
{: .bash}

### Running our first test

When we run `pytest`, it will look for directories and files which start with `test` or `test_`. CookieCutter has already created a test for us. Let's examine this file. In a text editor, create a file called `test_geometry.py`, and add a description to the top. We are working with pytest, so we will also import the package.

~~~
"""
Unit and regression test for our molecular geometry functions
"""
~~~

To create a test, we define a function starting with `test_`. For example, to create a test for the code we've been running by hand to test our functions, our file would change to

~~~
"""
Unit and regression test for our molecular geometry functions
"""

import pytest
import geometry as gm
import numpy as np

def test_calculate_distance():
    r1 = np.array([0,0,0])
    r2 = np.array([0, 0.1, 0])

    calculated_distance = gm.calculate_distance(r1, r2)

    assert calculated_distance == 0.1
~~~
{: .language-python}

The last line, containing the python keyword `assert`, is called an assertion. Assertions are used to check the behavior of the code during runtime. The `assert` keyword halts code execution instantly if the comparison is False, and does nothing if the comparison is True.

In the terminal, run the test with 

~~~
$ pytest -v
~~~
{: .language-bash}

Here, `pytest` has looked through our directory and its subdirectories for anything matching `test*`. It found the `tests` folder, and within that folder, it found the file `test_molssi_devops.py`. It then executed the function `test_molssi_devops_imported` within that module. Since our `assertion` was True, the test passed.

We can see the names of the tests `pytest` ran by adding a `-v` tag to the pytest command.

Using the command argument ` -v` will result in pytest, listing which tests are executed and whether they pass or not. There are a number of
additional command line arguments to [explore](https://docs.pytest.org/en/latest/usage.html).

A simple test for the build_bond_function might look like this

~~~
def test_bond_list():
    coordinates = np.array([[0,0,0],[0, 1, 0],[0, 3, 0]])

    expected_result = {(0,1) : 1.}

    calculated_result = gm.build_bond_list(coordinates)

    assert list(calculated_result.keys()) == list(expected_result.keys())
    assert list(calculated_result.values()) == list(expected_result.values())
~~~

### Failing tests
Let's see what happens when one of the test fails.

In case of test failure, Pytest will show detailed output from doing its own analysis to discover the error by inspecting your objects at runtime. Change the value of the `expected` variable in your test function for the distance to `2` and rerun the test.

~~~
$ pytest -v
~~~
{: .language-bash}

Pytest shows a detailed failure report, including the source code around the failing line. The line that failed is marked with `>`.
Next, it shows the values used in the assert comparison at runtime, that is `0.1 == 2`. This runtime analysis is one of the advantages of pytest that help you debug your code.

Change the expected value back to 0.1 so that your tests pass.

> ## Test Driven Development - TDD
> Sometimes, tests are written before code is actually written. This is called "Test Driven Development" or TDD. In this case, you would write tests which define the behavior of your code, run the tests to see they pass, then write code to pass each test. TDD is common when developing a library with well-defined interfaces and features.
{: .callout}

### Checking inputs - raising errors

Sometimes you may want to raise errors or modify errors Python will already raise to give more 
helpful error messages.

Perhaps we would like to modify our `build_bond_list` function to only accept arrays of a certain shape. Consider the following code

~~~
>>> import geometry as gm
>>> import numpy as np
>>> rand_coordinates = np.random.random([3, 10, 3])
>>> gm.build_bond_list(rand_coordinates)
{}
~~~
{: .language}

Here, we have passed our function an array. The code has not failed, but we do not have any bonds.

Let's modify this function to check the shape of the input array. If it has more than 2 dimensions, we will raise an error.

~~~
def build_bond_list(coordinates, max_bond=1.5, min_bond=0):

    if len(coordinates).shape > 2:
        raise ValueError('Input coordinate array should have fewer than three dimensions')
~~~
{: .language-python}

To practice these concepts, let's define another function (mean), which can calculate the mean of a list, just to examine some raising errors and other features of pytest. You can't use `np.mean` here.

~~~
def mean(num_list):
    """
    Calculate the mean/average of a list of numbers.

    Parameters
    ----------
    num_list : list
        The list to take the average of

    Returns
    -------
    mean_list: float
        The mean of the list

    Examples
    --------
    >>> mean([1, 2, 3, 4, 5])
    3.0
    """

    mean_list = sum(num_list)/len(num_list)

    return mean_list
~~~

What are some thing we might want to check on user input for the `mean` function? If you think for a bit, you may come up with the following responses:

1. Check that input is type `list`
2. Check that input list has length (ie that it is not an empty list)
3. Check that all elements of the input list are numeric.

> ## Exercise
> Modify your mean function to check that inputs meet all three criteria outlined above.
>> ## Solution
>> ~~~
>> def mean(num_list):
>>    """
>>    Computes the mean of a list of numbers.
>>
>>    Parameters
>>    ----------
>>    num_list: list
>>        List to calculate mean of
>>
>>    Returns
>>    -------
>>    list_mean: float
>>    Mean of list of numbers
>>    """
>>
>>     # Check that input is type list
>>    if not isinstance(num_list, list):
>>        raise TypeError('Invalid input %s - must be type list' %(num_list))
>>
>>    # Check that list is not empty
>>    if not num_list:
>>        raise ValueError('Cannot calculate the mean of an empty list.')
>>
>>    try:
>>        list_mean = sum(num_list) / len(num_list)
>>    except TypeError:
>>        raise TypeError('Cannot calculate mean of list - all list elements must be numeric')
>>
>>    return list_mean
>> ~~~
>> {: .language-python}
>>
> {: .solution}
{: .challenge}

We can modify our tests to check that all of the expected exceptions are raised.

Add the following two tests to `test_molssi_math.py`
~~~
def test_mean_type_error():
    test_variable = 'this is a string'

    with pytest.raises(TypeError):
        molssi_devops.mean(test_variable)

def test_zero_length():
    test_list = []

    with pytest.raises(ValueError):
        molssi_devops.mean(test_list)
~~~
{: .language-python}


### Testing Expected Exceptions

If you expect your code to raise exceptions, you can test this behavior with pytest.
First, you need to import `pytest`. 
We can modify our tests to check that all of the expected exceptions are raised.

Add the following two tests to `test_geom.py`
~~~
def test_mean_type_error():
    test_variable = 'this is a string'

    with pytest.raises(TypeError):
        molssi_devops.mean(test_variable)

def test_zero_length():
    test_list = []

    with pytest.raises(ValueError):
        molssi_devops.mean(test_list)
~~~
{: .language-python}

### More Pytest Features - Pytest Marks

Marks are an easy way to add annotations to your tests. For instance, you can mark some tests to be skipped
by adding the skip decorator to your test method `pytest.mark.skip`.
Add the following code to your `test_geom.py`.

~~~
@pytest.mark.skip
def test_calculate_distance():
~~~
{: .python}

after running `pytest -v`, pytest will run the other tests, skipping test_calculate_distance.
~~~

## Edge and Corner Cases

### Edge cases
The situation where the test examines either the beginning or the end of a range, but not the middle, is called an edge case.
In a simple, one-dimensional problem, the two edge cases should always be tested along with at least one internal point.
This ensures that you have good coverage over the range of values.

Anecdotally, it is important to test edges cases because this is where errors tend to arise. Qualitatively different behavior happens at boundaries. As such, they tend to have special code dedicated to them in the implementation.

### Corner cases
When two or more edge cases are combined, it is called a corner case. If a function is parametrized by two linear and independent variables, a test that is at the extreme of both variables is in a corner.

## Advanced features of pytest (fixtures, parameterize)

### Pytest Fixtures

Fixtures are resources that tests can repeatedly request to use. Fixtures can be used for dependency injection (a way of passing or supplying resources from one object to another) which help decouple the code and make it cleaner.

To use fixtures, we need to import `pytest`. Fixtures can be defined as methods, where the name of the method is the name of this resource, and
the returned data is its value. For this example:

~~~
@pytest.fixture
def num_list_3():
    return [1, 2, 3, 4, 5]
~~~
{: .python}

we defined a fixture named `num_list_3` which will have the value `[1, 2, 3, 4, 5]`. Now, any test
method can request this fixture by adding its name to its input argument as follows.

~~~
def test_mean(num_list_3):
    assert mean(num_list_3) == 3.0
~~~
{: .python}

Fixtures can be reused by other tests too. Also, test methods can request multiple fixtures.

For testing our geometry analysis, recall that we initially compared our answers to a result from QCArchive. We can use a pytest fixture to pull down that structure from QCArchive.

~~~
@pytest.fixture
def butane_molecule():
    client = ptl.FractalClient()
    butane_molecules = client.query_molecules(id=['61139', '70659'])

    yield butane_molecules
~~~

## Exercise
Write a function `test_butane_bonds` which calculates the bonds for the butane molecule from QCArchive and compares your result with the known result stored in QCArchive.

~~~
def test_butane_bonds(butane_molecule):

    angstrom_to_bohr = qcelemental.constants.conversion_factor("angstrom", "bohr")
    max_length = 1.55 * angstrom_to_bohr

    known_bonds = butane_molecule[0].connectivity
    calculated_bonds = geom.build_bond_list(butane_molecule[0].geometry.copy(), max_bond=max_length)
    calculated_keys = list(calculated_bonds.keys())

    assert len(known_bonds) == len(calculated_bonds)

    for i in range(len(known_bonds)):
        assert known_bonds[i][:2] == calculated_keys[i]
~~~

### Pytest Parametrize

The built-in `pytest.mark.parametrize` decorator enables parametrization of arguments for a test function.
Here is a typical example of a test function that implements checking that a certain input leads to an expected output.

~~~
import pytest
import numpy as np

@pytest.mark.parametrize("num_list, expected_mean" , [
    ([1, 2, 3, 4, 5], 3),
    ([0, 2, 4, 6], 3),
    ([1, 2, 3, 4], 2.5),
    (list(range(1, 1000000)), 1000000/2.0)
])

def test_many(num_list, expected_mean):
    # assert mean(num_list) == expected_mean
    assert np.isclose(mean(num_list), expected_mean, 1e-6)
~~~
{: .python}

Here, the @parametrize decorator defines four different (test_input, expected) tuples
so that the `test_many` function will run four times using them in turn.
Here, we used the `numpy` method `isclose` to compare float values within the range `1e-6`.

## Exercise

Write a parametrize function to check the calculate distance function for many distances.

To get all combinations of multiple parametrized arguments you can stack parametrize decorators:

~~~
import pytest
@pytest.mark.parametrize("x", [0, 1])
@pytest.mark.parametrize("y", [2, 3])
def test_foo(x, y):
    pass
~~~
{: .python}

This will run the test with the arguments set to x=0/y=2, x=1/y=2, x=0/y=3,
and x=1/y=3 exhausting parameters in the order of the decorators.

