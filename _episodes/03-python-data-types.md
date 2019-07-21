---
title: "Python Data Types"
teaching: 20
exercises: 10
questions:
- "How can I store data in Python variables?"
objectives:
- "Be able to name and initialize different built-in data types of Python"
keypoints:
- "Numeric data types include integers (`int`), floating point numbers (`float`), and complex numbers."
- "Text is represented as a string (`str`)"
- "You can use lists, tuples, or dicitionaries to group data together"
- "Tuples and lists are similar, but tuples cannot be modified after they are created."
- "Python dictonaries use key-value pairs to group data together"
---

If you completed the material in the Python Data and Scripting workshop, you learned of a few data types. Those were `int` (integers), `float` (floating point numbers), `str` (strings), and `lists`.

This lesson will review those data types, and talk about a few additional ones we will be using throughout the week. 

## Numeric and Text Data Types - integers, floats, and strings
As a review, here is how you would initialize variables of each type. If a number does not have a decimal, it will be an integer. A string is created by surrounding a value with either single (`'`) or double (`"`) quotes, as shown below.

~~~
a = 1
print(F'The type of variable `a` is {type(a)}')

# A float is initialized with a decimal
b = 1.
print(F'The type of variable `b` is {type(b)}')

# A string is initialized with quotation marks 
c = '1'
print(F'The type of variable `c` is {type(c)}')
~~~
{: .language-python}

~~~
The type of variable `a` is <class 'int'>
The type of variable `b` is <class 'float'>
The type of variable `c` is <class 'str'>
~~~
{: .language-output}

Floats and integers are numeric data types. For example, in the code above, a and b can be added because they are both numeric.

~~~
a + b
~~~
{: .language-python}

~~~
2.0
~~~
{: .output}

However, you can not add a numeric and non-numeric type.

~~~
a + c
~~~
{: .language-python}

~~~
TypeError: unsupported operand type(s) for +: 'int' and 'str'
~~~
{: .language-output}

> ## Review
> How would you change the expression `a + c` so that the numeric values of `a` and `c` could be added?
>> ## Answer
>> This would require changing `c` to a numeric type. You could cast it as integer or float.
>> ~~~
>> a + int(c)
>> ~~~
>> {: .language-python}
>> You could have also used `float(c)` here. This will change the variable stored in `c` to a number if possible.
>> {: .output}
> {: .solution}
{: .challenge}

In addition to floats and integers, Python also supports complex numbers. We will not talk much about these, but you can create them by appending a 'j' or 'J' to a number.

~~~
# Create a complex number
complex_number = 1.0J
complex_number ** 2
~~~
{: .language-python}

## More about strings
Strings are created using double or single quotes.

~~~
my_string = 'This is a test string'
~~~
{: .language-python}

Recall that strings and numeric types cannot be added. For example,

~~~
a + c
~~~
{: .language-python}

will give a Type Error.

However, you can use the `+` operator on two strings. This results in string concatenation.

~~~
c = '1'
d = '0'
e = c + d
print(e)
~~~
{: .language-python}

~~~
'10'
~~~
{: .language-output}

> ## Check your understanding
> Predict the output of each print statement.
> ~~~
> print(10 + 20)
> print('10' + '20')
> print('10' + 20)
> ~~~
> {: .language-python}
>> ## Answer
>> ~~~
>> 30
>> 1020
>> TypeError: can only concatenate str (not "int") to str
>> ~~~
>> {: .output}
>> The first statement added two numeric types, meaning addition was performed.
>> For the second statement, the `+` operator was performed on two strings, meaning that the strings were concatenated.
>> The third statement results in a TypeError, beause a string and numeric type cannot be added.
>>{: .output}
> {: .solution}
{: .challenge}

### Accessing string elements
Strings can be indexed by using square brackets, and giving an element number (similar to lists). For example, to access the first element in the string

~~~
print(F'The first letter in the string is {my_string[0]}')
~~~
{: .language-python}

~~~
T
~~~
{: .language-python}

You can also check if strings contain substrings using the following syntax.

~~~
'substring' in 'string'
~~~
{: .language-python}

For example, 
~~~
print(my_string)
print('test' in my_string)
~~~
{: .language-python}

~~~
'This is a test string'
True
~~~
{: .language-python}

## Grouping data together - lists, tuples, and dictionaries

### Lists
There are several built-in data structures in Python which can be used to group similar data together. One commonly used data type which was covered extensively in the Python Data and Scripting Lesson is the `list`.

~~~
# A list is initialized with square brackets
my_list = []

# When elements are present in a list, they are separated by commas
my_list = [1, 2, 3, 4]
~~~
{: .language-python}

List has several built-in methods. These methods are accessed by adding a dot (`.`) and the method name after the list. In Python, we can see all of the methods associated with an object by using the `dir` command.

~~~
dir(my_mixed_list)
~~~
{: .language-python}

~~~
['__add__',
 '__class__',
 '__contains__',
 '__delattr__',
 '__delitem__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getitem__',
 '__gt__',
 '__hash__',
 '__iadd__',
 '__imul__',
 '__init__',
 '__init_subclass__',
 '__iter__',
 '__le__',
 '__len__',
 '__lt__',
 '__mul__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__reversed__',
 '__rmul__',
 '__setattr__',
 '__setitem__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 'append',
 'clear',
 'copy',
 'count',
 'extend',
 'index',
 'insert',
 'pop',
 'remove',
 'reverse',
 'sort']
~~~
{: .language-output}

Pay attention to the methods at the bottom which do not begin with `__`. These are methods which you can use on a list variable (the others are special methods we will learn about later).

In particular, you will see `append`, which we used in the Python Data and Scripting workshop. Recall that `append` adds another list element at the end of the list. 

There are several other methods as well. These methods will act on the list, and may modify the list. For example, the `.sort()` method will modify the list in place to sort values from lowest to highest.

~~~
numeric_list = [1, 2, 100, 20, 5]
print(F'Unsorted list : {numeric_list}')
numeric_list.sort()
print(F'Sorted list : {numeric_list}')
~~~
{: .language-python}

~~~
[1, 2, 100, 20, 5]
Sorted list : [1, 2, 5, 20, 100]
~~~
{: .output}

### Tuples
Tuples are another data type which seem very similar to lists. 

~~~
# A tuple is initialized with parenthesis, with values separated by commas
my_tuple = (1, 2, 3, 4)
~~~
{: .language-python}

~~~
dir(my_tuple)
~~~
{: .language-python}

~~~
['__add__',
 '__class__',
 '__contains__',
 '__delattr__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getitem__',
 '__getnewargs__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__iter__',
 '__le__',
 '__len__',
 '__lt__',
 '__mul__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__rmul__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 'count',
 'index']
 ~~~
 {: .output}

 Tuples have much fewer methods associated with them.

However, unlike lists, tuples cannot be changed after being created.


~~~
my_tuple[0] = 0
~~~
{: .language-python}

~~~
TypeError: 'tuple' object does not support item assignment
~~~
{: .language-output}

#### Iteration - Tuples and Lists
You can iterate through the items in both tuples and lists using a `for` loop.

Let's consider this block of code.

~~~
# This is a list
energy_kcal = [-13.4, -2.7, 5.4, 42.1]
# I can determine its length
energy_length = len(energy_kcal)

# print the list length
print('The length of this list is', energy_length)
~~~
{: .language-python}

> ## Exercise
> Use a `for` loop to iterate through the variable `energy_kcal`. Create a new list, `energy_kJ` which contains the values in kJ. Hint: 1 kJ = 4.184* (1 kcal)
>
>> ## Answer
>> ~~~
>> energy_kJ = []
>> for value in energy_kcal:
>>      energy_kJ.append(value*4.184)
>> ~~~
>> {: .language-python}
>> We first create an empty list (`energy_kJ`), then iterate through the `energy_kcal` list using a `for` loop. We append each new calculated value to `energy_kJ`. 
>> {: .outout}
> {: .solution}
{: .challenge}

### Dictionaries

The last data type we will discuss is dictionaries. Dictionaries are data structures which allow you to store values using `key, value` pairs.

~~~
# An empty dictionary is initialized with curly braces.
my_dictionary = {}
type(my_dictionary)
~~~
{: .language-python}

We can use a dictionary to group data together. 

~~~
benzene_molecule = {
    'name' : 'benzene',
    'formula' : 'C6H6',
    'molecular_weight' : 78.11,
}
~~~
{: .language-python}

You create data in a dictionary when the dictionary is initialized by first naming a key, then its value separated by a colon (`:`). We can access data associated with the keys by using the dictionary name and the key of interest in square brackets. For example, to access the formula of `benzene_molecule`,

~~~
benzene_molecule['formula']
~~~
{: .language-python}

~~~
'C6H6'
~~~
{: .output}

> ## Check your understanding
> How would you access the molecular weight of our benzene molecule?
>> ## Answer
>> ~~~
>> benzene_molecule['molecular_weight']
>> ~~~
>> {: .language-python}
>> ~~~
>> 78.11
>> ~~~
>> {: .language-output}
>> We access the molecular weight by using the "molecular_weight" keyword.
>> {: .output}
> {: .solution}
{: .challenge}

If we want to add another key value pair to an existing dictionary, we do so in a way similar to assigning a variable, except that we put the new keyword in square brackets after the dictionary name.

~~~
benzene_molecule['melting_point'] = 5.5
print(benzene_molecule)
~~~
{: .language-python}

~~~
{'name': 'benzene', 'formula': 'C6H6', 'molecular_weight': 78.11, 'melting_point': 5.5}
~~~
{: .language-output}

> ## Exercise
> Consider the following block of code which defines a list of molecules and their molecular weights.
> ~~~
> # Define molecules
> molecules = ["methane", "ethane", "benzene", "propane", "toluene", "butane", "ethylene"]
> weights = [16.04, 30.07, 78.11, 44.1, 91.14, 58.12, 28.05]
> ~~~
> {: .language-python}
> 
> Convert this code to use a dictionary (`molecular_weights`) where the keys are molecule names, and the values are molecular weights. 
>> ## Answer
>> ~~~
>> molecular_weight = {}
>> alkanes = {}
>>
>> for i in range(len(molecules)):
>>     molecular_weight[molecules[i]] = weights[i]
>>
>> print(molecular_weight)
>> ~~~
>> {: .language-python}
>> ~~~
>> {'methane': 16.04, 'ethane': 30.07, 'benzene': 78.11, 'propane': 44.1, 'toluene': 91.14, 'butane': 58.12, 'ethylene': 28.05}
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

We can also access all of the keys or values of the dictionary:

~~~
benzene_keys = list(benzene_molecule.keys())
benzene_values = list(benzene_molecule.values())

print(F'The dictionary keys are {benzene_keys}')
print(F'The diciontary values are {benzene_values}')
~~~

## Looping through dictionaries
There are a few ways to iterate through dictionaries. 

If we iterate through them in the same way as lists, we access the dictionary keys.

~~~
for k in benzene_molecule:
    print(k)
~~~
{: .language-python}

~~~
name
formula
molecular_weight
melting_point
~~~
{: .output}

We can iterate through key, value pairs by adding `.items()` to the end of the dictionary.
~~~
for k,v in molecule.items():
    print(k,v)
~~~
{: .language-python}

> ## Check your understanding
> Using the dictionary from the previous exercise (`molecular_weight`), create a second dictionary (`alkanes`) which only has molecular weights for alkanes.
> > ## Answer
>> ~~~
>> alkanes = {}
>>
>> for k,v in molecular_weight.items():
>>    if 'ane' in k:
>>         alkanes[k] = v
>> 
>> print(alkanes)
>> ~~~
>> {: .language-python}
>> ~~~
>> {'methane': 16.04, 'ethane': 30.07, 'propane': 44.1, 'butane': 58.12}
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

## Copying variables

Imagine that you wanted to do some kind of mathematical manipulation on your new dictionary `alkanes` - maybe you wanted to convert it to kilograms per mol.

You write the following code to do this operation, starting with making a copy of the `alkanes` dictionary.

~~~
alkanes_kilograms = alkanes

for k,v in alkanes_kilograms.items():
    alkanes_kilograms[k] = v / 1000

print(alkanes_kilograms)
~~~
{: .language-python}

~~~
{'methane': 0.01604, 'ethane': 0.03007, 'propane': 0.0441, 'butane': 0.05812}
~~~
{: .output}

Great! It looks like it behaved exactly the way we wanted. However, our calculation had some unexpected consequences. Let's check out our original dictionary.

~~~
print(alkanes)
~~~
{: .language-python}

~~~
{'methane': 0.01604, 'ethane': 0.03007, 'propane': 0.0441, 'butane': 0.05812}
~~~
{: .output}

Modifying our 'copy' of the dictionary has actually modified the original dictionary. That's not what we wanted at all!

In Python, when you set variables equal to other mutable variables like lists or dictionaries, the original is modified.

The solution is to use a copy function. On a list or dictionary, we can do this using a `.copy` function.

First, let's revert our dictionary back to the original.

~~~
for k,v in alkanes_kilograms.items():
    alkanes_kilograms[k] = v * 1000

print(alkanes_kilograms)
print(alkanes)
~~~
{: .language-python}

~~~
{'methane': 16.04, 'ethane': 30.07, 'propane': 44.1, 'butane': 58.12}
{'methane': 16.04, 'ethane': 30.07, 'propane': 44.1, 'butane': 58.12}
~~~
{: .output}

To make a true copy of our dictionary, use the `.copy` function.

~~~
alkanes_kilograms = alkanes.copy()

for k,v in alkanes_kilograms.items():
    alkanes_kilograms[k] = v * 1000

print(alkanes_kilograms)
print(alkanes)
~~~
{: .language-python}

~~~
{'methane': 16040.0, 'ethane': 30070.0, 'propane': 44100.0, 'butane': 58120.0}
{'methane': 16.04, 'ethane': 30.07, 'propane': 44.1, 'butane': 58.12}
~~~
{: .output}

> ## Exercise
> Try each of the following commands. When do you need to make copies of variables?
> ~~~
> # What happens to `b` if you modify `a`?
> a = [1, 2, 3]
> b = a
> ~~~
> {: .language-python}
> ~~~
> # What happens to `c` if you modify `a`?
> a = [1, 2, 3]
> c = [[1, 2, 3], [4, 5, 6]]
> c[0][:] = a
> ~~~
> {: .language-python}
>> ## Answer
>> For the first code block, changing `a` also changes `b`.
>>
>> For the second code block, changing `a` does not change `c`.
> {: .solution}
{: .challenge}

{% include links.md %}

