---
title: "Working with Numpy Arrays"
teaching: 40
exercises: 20
questions:
- "What are the differences between numpy arrays and lists?"
- "How can I use NumPy to do calculations?"
objectives:
- "Be able to name the differences between Python lists and numpy arrays."
- "Understand the idea of broadcasting"
- ""
keypoints:
- "NumPy arrays which are the same size use element-wise operations when added or subtracted"
- "NumPy uses something called *broadcasting* for arrays which are not the same size to allow arrays to be added or multiplied."
- "NumPy has several functions to create arrays such as linspace (linearly spaced array) and zeros (create an array of 0's)"
---

[Numpy](https://numpy.org/) is a widely used Python library for scientific computing. It has a number of useful features, including the a data structure called an array which we will be using in our domain projects througout the summer school.

## NumPy Arrays vs. Python Lists

Previously, you have worked with the built-in types of `lists`. NumPy arrays seem similar, but offer some distinct advantages. Numpy arrays take up less space, are faster, and have more associated mathematical operations associated with them. However, unlike lists, they elements all have to be the same type.

There are also differences in how lists and numpy arrays behave. Let's look at some of these.

To use the numpy library, we have to import it. When `numpy` is imported, it is often shortened to `np`.

~~~
import numpy as np

# Create some lists
r1 = [0.0, 0.1, 0.0]
r2 = [0.0, 0.0, 0.0]

# Create some numpy arrays
r1_array = np.array(r1)
r2_array = np.array(r2)
~~~
{: .language-python}

In this code block, we've created two lists (`r1` and `r2`). We then created numpy array versions of these lists.

When we print `r1` and `r1_array`, the results look very similar.

~~~
print(r1)
print(r1_array)
~~~
{: .language-python}

~~~
[0.0, 0.1, 0.0]
[0.  0.1 0. ]
~~~
{: .output}

However, one way numpy arrays and lists vary significantly is that you can easily perform element-wise operations on numpy arrays without loops. You can add two arrays together, multiply arrays by scalars, or do element-wise multiplcation of arrays.

For example, you can multiply every element in a numpy array by 10

~~~
print(10*r1_array)
~~~
{: .language-python}

~~~
[0. 1. 0.]
~~~
{: .output}

or, you can multiply two numpy arrays to get their element-wise product.

But if you multiply r1 (which is a list), by 10, the list will just be repeated 10 times.

~~~
print(r1*10)
~~~
{: .language-python}

~~~
[0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0, 0.0, 0.1, 0.0]
~~~
{: .output}

> ## Check your understanding
> What does the following code result in?
>
> ~~~
> a1 = np.array([2, 1, 0])
> a2 = np.array([1, 3, 5])
> 
> print(a1 * a2)
> ~~~
> {: .language-python}
> 
> What happens if a1 and a2 are lists?
>
>> ## Answer
>> 
>> The code block results in 
>> ~~~
>> [2 3 0]
>> ~~~
>> {: .output}
>> The first element is `a1[0]*a2[0]`, the second element is `a1[1]*a2[1]`, and the third element is `a1[2]*a2[2]`.
>>
>> If a1 and a2 were lists instead, you would get a `TypeError`. `TypeError: can't multiply sequence by non-int of type 'list'`.
>>
> {: .solution}
{: .challenge}

## Broadcasting
The multiplication of `r1_array*10` is an example of what is called *broadcasting* in NumPy. This occurs when arrays have different shapes. If possible, the smaller array is "broadcast" across the larger array. For example, in

~~~
r1_array * 10
~~~
{: .language-python}

You can think of the scalar `10` being stretched during the operation to be the same size and shape as `r1_array`.


## Returning to the geometry analysis project

In the Python Data and Scripting Workshop, you had a homework assignment to analyze an xyz file, find the bonds, and print bond lengths.

Today, we will extend that project. We will look at using numpy for the bond length function, and writing a new function to analyze bond angles.

Recall that a solution given for a function for calculating distances between two points was the following

~~~
def calculate_distance(rA, rB):
    """Calculate distance between points A and B"""
    x_dist = (rA[0] - rB[0]) ** 2
    y_dist = (rA[1] - rB[1]) ** 2
    z_dist = (rA[2] - rB[2]) ** 2
    
    distance = np.sqrt(x_dist + y_dist + z_dist)
    
    return distance
~~~
{: .language-python}

This function would work for both lists and numpy arrays, because it does not assume that rA and rB can do something like element-wise subtraction. 

> ## Exercise
> Using what you've learned about numpy arrays, rewrite the `calculate_distance` function to use the features of numpy arrays.
>> ## Answer
>> If rA and rB are both numpy arrays, we can use element-wise subtraction
>> ~~~
>>  def calculate_distance(rA, rB):
>>     """Calculate the distance between points A and B"""
>>     AB = (rB - rA)**2
>>     distance = np.sqrt(np.sum(AB))
>>     return distance
>> ~~~
>> {: .language-python}
>> You might also have used the `numpy` function `np.linalg.norm` which calculates the magnitude of a given vector.
>> ~~~
>> def calculate_distance(rA, rB):
>>    """Calculate the distance between points A and B"""
>>    dist_vec = (rA-rB)
>>    distance = np.linalg.norm(dist_vec)
>>    return distance
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

Redefine your original distance function as `calculate_distance_list`. Using both, we see that both functions give the same answer. 

~~~
print(calculate_distance_list(r1, r2))
print(calculate_distance(r1_array, r2_array))
~~~
{: .language-python}

## Some numpy functions - np.linspace, np.zeros, np.reshape
Let's try out our new function with more test data. We will need to generate an array of 3D points. We will need the dimension to be `(n, 3) where n is the number of atoms. 

There are many ways you could generate this test array. For the solution here, we'll generate 10 evenly-spaced 3 dimensional points.

First, we'll create a numpy array of evenly spaced points using the function `np.linspace`.

~~~
help(np.linspace)
~~~
{: .language-python}

First, create 30 evenly spaced points between 0 and 10.

~~~
test_array = np.linspace(0, 10, num=30)
print(F'The shape of our test array is {test_array.shape}')
print(F'Our array has {test_array.size} elements\n')
print(test_array)
~~~
{: .language-python}

~~~
The shape of our test array is (30,)
Our array has 30 elements

[ 0.          0.34482759  0.68965517  1.03448276  1.37931034  1.72413793
  2.06896552  2.4137931   2.75862069  3.10344828  3.44827586  3.79310345
  4.13793103  4.48275862  4.82758621  5.17241379  5.51724138  5.86206897
  6.20689655  6.55172414  6.89655172  7.24137931  7.5862069   7.93103448
  8.27586207  8.62068966  8.96551724  9.31034483  9.65517241 10.        ]
~~~
{: .output}

To visualize the data, type

~~~
import matplotlib.pyplot as plt
from mpl_toolkits import mplot3d

plt.plot(test_array)
~~~
{: .language-python}

Next, let's reshape this array so that it something that we're more used to looking at. Points in our function are assumed to have three parts (x, y, z). We have 30 points in test array, so we will want to reshape it to have 10 rows and 3 columns. We can do this by using the `reshape` function in numpy.

~~~
reshaped_array = test_array.reshape((10,3))
print(reshaped_array)

print(F'Our reshaped array has {reshaped_array.size} elements\n')
~~~
{: .language-python}

~~~
Our reshaped array has 30 elements.
[[ 0.          0.34482759  0.68965517]
 [ 1.03448276  1.37931034  1.72413793]
 [ 2.06896552  2.4137931   2.75862069]
 [ 3.10344828  3.44827586  3.79310345]
 [ 4.13793103  4.48275862  4.82758621]
 [ 5.17241379  5.51724138  5.86206897]
 [ 6.20689655  6.55172414  6.89655172]
 [ 7.24137931  7.5862069   7.93103448]
 [ 8.27586207  8.62068966  8.96551724]
 [ 9.31034483  9.65517241 10.        ]]
~~~
{: .output}

We can also graph this on a 3D axis.

~~~
ax = plt.axes(projection='3d')
ax.scatter3D(reshaped_array[:,0], reshaped_array[:,1], reshaped_array[:,2])
~~~
{: .language-python}

Now, instead of having a 1 dimensional array of 30 points which go from 0 to 10, we have a 3 dimensional array of 10 points which go from (0, 0.34482759, 0.68965517) to (9.31034483, 9.65517241, 10.). Let's use these points in our distance function to make sure that neighboring points are evenly spaced.

~~~
for i in range(len(reshaped_array)-1):
    p1 = reshaped_array[i]
    p2 = reshaped_array[i+1]
    distance = calculate_distance(p1,p2)
    print(F'{i} to {i+1} : {distance}')
~~~
{: .language-python}

~~~
0 to 1 : 1.7917766974850455
1 to 2 : 1.7917766974850455
2 to 3 : 1.7917766974850458
3 to 4 : 1.7917766974850455
4 to 5 : 1.7917766974850458
5 to 6 : 1.7917766974850458
6 to 7 : 1.7917766974850453
7 to 8 : 1.7917766974850462
8 to 9 : 1.7917766974850446
~~~
{: .output}

We see that we have 10 evenly spaced points.

> ## Check your understanding
> Edit your code so that the answers are stored in a Python dictionary where the keys are the labels '0 to 1'... and the values are the distances.
>> ## Answer
>>
>> ~~~
>> # Create empty dictionary
>> distances = {}
>> for i in range(len(reshaped_array)-1):
>>     p1 = reshaped_array[i]
>>     p2 = reshaped_array[i+1]
>>     distance = calculate_distance(p1,p2)
>>     distances[F'{i} to {i+1}'] = distance
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

More often, what you might want to do store the answer in a numpy array. If you know the size of your array, you can preallocte it using `np.zeros`.

~~~
distances = np.zeros(len(reshaped_array)-1)

for i in range(len(reshaped_array)-1):
    p1 = reshaped_array[i]
    p2 = reshaped_array[i+1]
    distance = calculate_distance(p1,p2)
    distances[i] = distance

print(distances)
~~~
{: .language-python}

~~~
[1.7917767 1.7917767 1.7917767 1.7917767 1.7917767 1.7917767 1.7917767
 1.7917767 1.7917767]
~~~
{: .output}

> ## Exercise
> Can you think of a way to create 10 evenly spaced points which go from (0, 0, 0) to (10, 10 , 10) using NumPy?
>> ## Answer
>> This is one solution, though there may be others.
>> ~~~
>> # Create an array of zeros of the desired size.
>> fill_array = np.zeros([10, 3])
>>
>> fill_array[:,0] = dim
>> fill_array[:,1] = dim
>> fill_array[:,2] = dim
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

**Make a point about slicing and copying**

This has been a general overview of some of the things that are possible with NumPy. NumPy is very powerful has many more functionalities that you can use. For a better idea of these, check out the [NumPy Documentation](https://numpy.org/). 