---
title: Basic Notes on NumPy
date: 2019-02-28
tags: [python]
---

## About NumPy

​	NumPy is the fundamental package for scientific computing with Python. It contains among other things:

- a powerful N-dimensional array object
- sophisticated (broadcasting) functions
- tools for integrating C/C++ and Fortran code
- useful linear algebra, Fourier transform, and random number capabilities

## The Basics

​	NumPy’s array class is called `ndarray`. It is also known by the alias `array`. In NumPy dimensions are called *axes*.

<!-- more -->

​	Note that `numpy.array` is not the same as the Standard Python Library class `array.array`, which only handles one-dimensional arrays and offers less functionality.

​	Here are some attributes of an `ndarray` object:

- **ndarray.ndim**

  The **number of axes** (dimensions) of the array.

- **ndarray.shape**

  The **dimensions** of the array. 

  This is a tuple of integers indicating the size of the array in each dimension. For a matrix with *n* rows and *m* columns, `shape` will be `(n,m)`. The length of the `shape` tuple is therefore the number of axes, `ndim`.

- **ndarray.size**

  The **total number of elements** of the array. This is equal to the product of the elements of `shape`.

- **ndarray.itemsize**

  The **size in bytes of each element** of the array. 

  For example, an array of elements of type `float64` has `itemsize` 8 (=64/8), while one of type `complex32` has `itemsize` 4 (=32/8). It is equivalent to `ndarray.dtype.itemsize`.

- **ndarray.dtype**

  An object describing the **type of the elements** in the array. 

An Example:

```python
>>> import numpy as np
>>> a = np.arange(15).reshape(3, 5)
>>> a
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
>>> a.shape
(3, 5)
>>> a.ndim
2
>>> a.dtype.name
'int64'
>>> a.itemsize
8	# per item
>>> a.size
15	# number of items
>>> type(a)
<type 'numpy.ndarray'>
```

## Array Creation

​	You can create an array from a regular Python list or tuple using the `array` function. The type of the resulting array is deduced from the type of the elements in the sequences.

A frequent error consists in calling `array` with multiple numeric arguments, rather than providing a single list of numbers as an argument.

```python
>>> a = np.array(1,2,3,4)    # WRONG
>>> a = np.array([1,2,3,4])  # RIGHT
```

The type of the array can also be explicitly specified at creation time:

```python
>>> c = np.array( [ [1,2], [3,4] ], dtype=complex )
>>> c
array([[ 1.+0.j,  2.+0.j],
       [ 3.+0.j,  4.+0.j]])
```

The function `zeros` creates an array full of zeros, the function `ones` creates an array full of ones, and the function `empty` creates an array whose initial content is random and depends on the state of the memory. By default, the dtype of the created array is `float64`.

```python
>>> np.zeros( (3,4) )
array([[ 0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.]])
>>> np.ones( (2,3,4), dtype=np.int16 )                
array([[[ 1, 1, 1, 1],
        [ 1, 1, 1, 1],
        [ 1, 1, 1, 1]],
       [[ 1, 1, 1, 1],
        [ 1, 1, 1, 1],
        [ 1, 1, 1, 1]]], dtype=int16)
```

To create sequences of numbers, NumPy provides a function analogous to `range` that returns arrays instead of lists.

```python
>>> np.arange( 10, 30, 5 )
array([10, 15, 20, 25])
>>> np.arange( 0, 2, 0.3 )	# it accepts float arguments
array([ 0. ,  0.3,  0.6,  0.9,  1.2,  1.5,  1.8])
```

For float data, use the function `linspace` that receives as an argument the number of elements that we want, instead of the step.

```python
>>> from numpy import pi
>>> np.linspace( 0, 2, 9 )	# 9 numbers from 0 to 2
array([ 0.  ,  0.25,  0.5 ,  0.75,  1.  ,  1.25,  1.5 ,  1.75,  2.  ])
>>> x = np.linspace( 0, 2*pi, 100 )	# useful to evaluate function at lots of points
>>> f = np.sin(x)
```

## Basic Operations

Unlike in many matrix languages, the product operator `*` operates elementwise in NumPy arrays. The matrix product can be performed using the `dot` function or method.

```python
>>> A = np.array( [[1,1],
...             [0,1]] )
>>> B = np.array( [[2,0],
...             [3,4]] )
>>> A * B                       # elementwise product
array([[2, 0],
       [0, 4]])
>>> A.dot(B)                    # another matrix product
array([[5, 4],
       [3, 4]])
```

Some operations, such as `+=` and `*=`, act in place to modify an existing array rather than create a new one.

```python
>>> a = np.ones((2,3), dtype=int)
>>> b = np.random.random((2,3))
>>> a *= 3
>>> a
array([[3, 3, 3],
       [3, 3, 3]])
>>> b += a
>>> b
array([[ 3.417022  ,  3.72032449,  3.00011437],
       [ 3.30233257,  3.14675589,  3.09233859]])
>>> a += b                  # b is not automatically converted to integer type
Traceback (most recent call last):
  ...
TypeError: Cannot cast ufunc add output from dtype('float64') to dtype('int64') with casting rule 'same_kind'
```

When operating with arrays of different types, the type of the resulting array corresponds to the more general or precise one (a behavior known as upcasting).

Many unary operations, such as computing the sum of all the elements in the array, are implemented as methods of the `ndarray`class.

- a.sum()
- a.min()
- a.max()

By default, these operations apply to the array as though it were a list of numbers, regardless of its shape. However, by specifying the `axis` parameter you can apply an operation along the specified axis of an array:

- a.max(axis=0)
- a.cumsum(axis=1)

Within NumPy, "universal functions" like sin, cos, and exp operate elementwise on an array, producing an array as output.

```python
>>> B = np.arange(3)
>>> B
array([0, 1, 2])
>>> np.exp(B)
array([ 1.        ,  2.71828183,  7.3890561 ])
>>> np.sqrt(B)
array([ 0.        ,  1.        ,  1.41421356])
```

## Indexing, Slicing and Iterating

**One-dimensional** arrays can be indexed, sliced and iterated over, much like Lists and other Python sequences.

**Multidimensional** arrays can have one index per axis. These indices are given in a tuple separated by commas:  
When fewer indices are provided than the number of axes, the missing indices are considered complete slices`:`  
The **dots** (`...`) represent as many colons as needed to produce a complete indexing tuple. For example, if `x` is an array with 5 axes, then  

- `x[1,2,...]` == `x[1,2,:,:,:]`,
- `x[...,3]` == `x[:,:,:,:,3]` 
- `x[4,...,5,:]` == `x[4,:,:,5,:]`.  

**Iterating** over multidimensional arrays is done with respect to the first axis.

## Shape Manipulation

1. Changing the shape of an array

   - a.ravel()	
   returns the array, flattened
   - a.reshape(m,n)	
   returns the array with a modified shape
   If a dimension is given as -1 in a reshaping operation, the other dimensions are automatically calculated.
   - a.T	
   returns the array, transposed  
2. Stacking together different arrays

   - np.vstack((a,b)) # Vertical
   - np.hstack((a,b)) # Horizontal
   - np.concatenate(a1,a2,...,axis=n) # Complex cases
3. Splitting one array into several smaller ones  

   - np.vsplit() # Vertical
   - np.hsplit() # Horizontal
   - np.array_split() # Complex cases

## Copies and Views

1. ### No Copy at All

   Simple assignments make no copy of array objects or of their data.

   Python passes mutable objects as references, so function calls make no copy.

2. ### View or Shallow Copy

   Different array objects can share the same data. The `view` method creates a new array object that looks at the same data.

   ```python
   >>> c = a.view()
   >>> c is a
   False
   >>> c.base is a	# c is a view of the data owned by a
   True
   >>> c.flags.owndata
   False
   >>>
   >>> c.shape = 2,6	# a's shape doesn't change
   >>> a.shape
   (3, 4)
   >>> c[0,4] = 1234	# a's data changes
   >>> a
   array([[   0,    1,    2,    3],
          [1234,    5,    6,    7],
          [   8,    9,   10,   11]])
   ```

   Slicing an array returns a view of it.

3. ### Deep Copy

   The `copy` method makes a complete copy of the array and its data.

> For less basic usage of NumPy, refer to the following documentation directly.

## References:

1. [NumPy Tutorial](https://docs.scipy.org/doc/numpy/user/quickstart.html)
2. [NumPy Documentation (Chinese Version)](https://www.numpy.org.cn/)