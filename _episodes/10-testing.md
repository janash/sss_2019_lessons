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
- "Enumerate the types of testing and the importance of each."
- "Explain pytest features and why pytest was selected."
---

Until now, we have been writing functions and checking their behavior using an interactive Python interpreter and manually inspecting the output. While this seems to work, it can be tedious and prone to error. In this lesson, we'll discuss how to write tests and run them using the `pytest` testing framework.

This episode explains the importance of code testing and demonstrates the possible capabilities.

## Why testing

Software should be tested regularly throughout the development cycle to ensure correct operation. Thorough testing is typically an afterthought, but for larger projects, it is essential for ensuring changes in some parts of the code do not negatively affect other parts.

Software testing is checking the behavior of part of the code (such as a method, class, or a module) by comparing its expected output or behavior with the observed one. We will explain this in more details shortly.


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
$ pip install -U pytest
~~~
{: .bash}

### Running our first test

When we run `pytest`, it will look for directories and files which start with `test` or `test_`. CookieCutter has already created a test for us. Let's examine this file. In a text editor, open `geometry_analysis/tests/test_geometry_analysis.py`

~~~
"""
Unit and regression test for the geometry_analysis package.
"""

# Import package, test suite, and other packages as needed
import geometry_analysis
import pytest
import sys

def test_geometry_analysis_imported():
    """Sample test, will always pass so long as import statement worked"""
    assert "geometry_analysis" in sys.modules
~~~
{: .language-python}

This file begins with `test_`, and contains a single function `test_geometry_analysis_imported`. This module will import our package, then checks to see if it has been imported correctly by checking if the package name is in the list of imported modules.

The last line, containing the python keyword `assert`, is called an assertion. Assertions are used to check the behavior of the code during runtime. The `assert` keyword halts code execution instantly if the comparison is False, and does nothing if the comparison is True.

We can see if this function works by running `pytest` in our terminal. In the top level of your package, run the following command.

~~~
$ pytest
~~~
{: .language-bash}

You should see an output similar to the following.

~~~
============================= test session starts ==============================
platform darwin -- Python 3.6.8, pytest-3.6.4, py-1.5.4, pluggy-0.6.0
rootdir: /Users/jessica/dev/molssi_devops, inifile:
collected 1 item

geometry_analysis/tests/geometry_analysis.py .                    [100%]

=========================== 1 passed in 0.06 seconds ===========================
~~~
{: .language-output}

Here, `pytest` has looked through our directory and its subdirectories for anything matching `test*`. It found the `tests` folder, and within that folder, it found the file `test_molssi_devops.py`. It then executed the function `test_molssi_devops_imported` within that module. Since our `assertion` was True, the test passed.

We can see the names of the tests `pytest` ran by adding a `-v` tag to the pytest command.

~~~
$ pytest -v
~~~
{: .language-bash}

Using the command argument ` -v` will result in pytest, listing which tests are executed and whether they pass or not. There are a number of
additional command line arguments to [explore](https://docs.pytest.org/en/latest/usage.html).

Using the `-v` argument, we see that `pytest` dsiplays the test name for us, as well as `PASSED` next to the test name.

## Testing our functions

We will now add tests to test our functions.

Create a new test in `test_geometry_analysis.py`.

~~~
def test_calculate_distance():
    
    r1 = np.array([0,0,-1])
    r2 = np.array([0, 1, 0 ])

    expected_distance = np.sqrt(2.)

    measured_distance = geometry_analysis.calculate_distance(r1, r2)

    assert measured_distance == expected_distance
~~~
{: .language-python}

We have written one test in this file. It calculates the mean of a test list, and asserts that it is equal to our expected value.

Run this test using `pytest`. In the terminal window, type

~~~
pytest -v
~~~

We now see that we have two tests which have been run, and they both passed.


### Failing tests
Let's see what happens when one of the test fails.

In case of test failure, Pytest will show detailed output from doing its own analysis to discover the error by inspecting your objects at runtime. Change the value of the `expected` variable in your test function to `3` and rerun the test.

~~~
$ pytest -v
~~~
{: .language-bash}

Pytest shows a detailed failure report, including the source code around the failing line. The line that failed is marked with `>`.
Next, it shows the values used in the assert comparison at runtime. This runtime analysis is one of the advantages of pytest that help you debug your code.

Change the expected value back to `2` so that your tests pass.

> ## Exercise
> Create a test for the `calculate_angle` function. 
>
>
> Verify that your test is working by running pytest. You should now see three passing tests.
>> ## Solution
>>
>> ~~~
>> def test_calculate_angle():
>>     r1 = np.array([1,0,0])
>>     r2 = np.array([0,0,0])
>>     r3 = np.array([0,1,0])
>>
>>     expected_value = 90
>>     calculated_value = geometry_analysis.calculate_angle(r1, r2, r3, degrees=True)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

> ## Test Driven Development - TDD
> Sometimes, tests are written before code is actually written. This is called "Test Driven Development" or TDD. In this case, you would write tests which define the behavior of your code, run the tests to see they pass, then write code to pass each test. TDD is common when developing a library with well-defined interfaces and features.
{: .callout}


~~~
def test_create_molecule():

    name = "water"
    symbols = ["H", "O", "H"]
    coordinates = np.array([[1, 0, 0], [0,0,0], [0, 1, 0]])

    water_molecule = geometry_analysis.Molecule(name, symbols, coordinates)

    assert water_molecule.name == name
    assert water_molecule.symbols == symbols
    assert np.array_equal(coordinates, water_molecule.coordinates)
~~~

### Testing Expected Exceptions

If you expect your code to raise exceptions, you can test this behavior with pytest.
First, you need to import `pytest`. We can test that an exception is properly raised when we input the wrong type to our `title_case` function.

In your `test_geometry_analysis.py` file, add the following.

~~~
def test_create_failure():

    name = 25
    symbols = ["H", "O", "H"]
    coordinates = np.array([[2, 0, 0], [0,0,0], [-2, 0, 0]])

    with pytest.raises(TypeError):
        water_molecule = geometry_analysis.Molecule(name, symbols, coordinates)
~~~
{: .language-python}

The test will pass if the `title_case` method raises a 'TypeError', otherwise, the test will fail.

### More Pytest Features - Pytest Marks

Marks are an easy way to add annotations to your tests. For instance, you can mark some tests to be skipped
by adding the skip decorator to your test method `pytest.mark.skip`.

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
def water_molecule():
    name = "water"
    symbols = ["H", "O", "H"]
    coordinates = np.array([[2, 0, 0], [0,0,0], [-2, 0, 0]])

    water = geometry_analysis.Molecule(name, symbols, coordinates)

    return water
~~~
{: .python}

~~~
def test_molecule_set_coordinates(water_molecule):
    """Test that our setter for coordinates works."""

    num_bonds = len(water_molecule.bonds)
    assert(len(water_molecule.bonds.keys()) == 2)
    
    new_coordinates = np.array([[5, 0, 0], [0,0,0], [0, 1, 0]])
    water_molecule.coordinates = new_coordinates
    assert(len(water_molecule.bonds.keys()) == 1)

    assert np.array_equal(new_coordinates, water_molecule.coordinates)

~~~
{: .language-python}

Fixtures can be reused by other tests too. Also, test methods can request multiple fixtures.


### Pytest Parametrize

The built-in `pytest.mark.parametrize` decorator enables parametrization of arguments for a test function.
Here is a typical example of a test function that implements checking that a certain input leads to an expected output.

~~~
@pytest.mark.parametrize("p1, p2, expected_distance", [
    (np.array([0, 0, 0]), np.array([0, 0, 1]), 1),
    (np.array([0, 0, 0]), np.array([0, 1, 1]), np.sqrt(2)),
    (np.array([-3, -2, -1]), np.array([3, 2, 1]), np.sqrt(6**2 + 4**2 + 2**2))
])
def test_distance_many(p1, p2, expected_distance):
    calculated_distance = geom.calculate_distance(p1, p2)

    assert calculated_distance == expected_distance

~~~
{: .python}

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

## Testing from QCArchive

~~~
@pytest.fixture
def butane_molecule():
    client = ptl.FractalClient()
    butane_molecules = client.query_molecules(id=['61139', '70659'])

    yield butane_molecules
~~~
{: .language-python}

~~~
def test_butane_bonds(butane_molecule):

    my_molecule = geometry_analysis.Molecule("butane", butane_molecule[0].symbols, butane_molecule[0].geometry )

    known_bonds = butane_molecule[0].connectivity

    calculated_bonds = my_molecule.bonds
    calculated_keys = list(my_molecule.bonds.keys())

    assert len(known_bonds) == len(calculated_bonds)

    for i in range(len(known_bonds)):
        assert known_bonds[i][:2] == calculated_keys[i]

def test_butane_distance(butane_molecule):

    coordinates = butane_molecule[0].geometry

    calculated_distance = geometry_analysis.calculate_distance(coordinates[0], coordinates[1])

    expected_distance = butane_molecule[0].measure([0, 1])

~~~
{: .language-python}

### Code Coverage Pt. 1

Now that we have a set of modules and associated tests, we want to see how much of our package is "covered" by our tests.
We'll measure this by counting the lines of our packages that are touched, i.e. used, during our tests.

We already have everything we need for this since we installed `pytest-cov` earlier which includes the coverage tools on top of the `pytest` package.

We can assess our code coverage as follows:

~~~
pytest --cov=geometry_analysis
~~~
{: .language-bash}

The output shows how many statements (i.e. not comments) are in a file, how many weren't executed during testing, and the percentage of statements that were.

To improve our coverage, we also want to see exactly which lines we missed and we can determine this using the `.coverage` file produced by `pytest`.
Unfortunately, this strategy becomes impractical when we are working with anything larger than our test package because the `.coverage` file becomes too convoluted to read.
We will need more tools to help us determine how to improve out tests and that will be the subject of Code Coverage pt. 2, which we will cover in the next Episode.

> ## Do we need to get 100% coverage?
>
> Short answer: __no__.
> Code coverage is a useful tool to assess how comprehensive our set of tests are and in general the higher our code coverage the better.
> __However__, trying to achieve 100% coverage on packages any larger than this sample package is a bit unrealistic and would require more time than that last bit of coverage is worth.
>
{: .callout}
