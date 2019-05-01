---
title: Tricks for Python
date: 2019-03-12
tags: [python]
---

Here are some tricks useful (to Python beginners like me) when writing Python code.

## Details about some built-in functions

1. **abs(x)**

   When the argument `x` is a complex number, this function will return the magnitude of `x`. 

   Here is an example: 

   - For `x=1+2j` , `abs(x)` will get $$ \sqrt{2} $$

2. **all(A); any(A)**

   Return `True` if all/any element of the *iterable* A is true.

3. **format()**

   It can be used this way to replace `bin()`

   ```python
   >>> format(14, 'b')
   1110
   ```
   A typical use of `format` goes like this:

   ```python
   >>> x = 10
   >>> y = -5.0
   >>> "x={:d},y={:.2f}".format(x,y))
   x=10,y=-5.00
   ```

<!-- more --> 

4. **enumerate(*iterable*, *start=0*)**

   It can be easily used to get the index of an item in a For-expression to replace `zip()`. 

   Here is an example:

   ```python
   >>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
   >>> list(enumerate(seasons))
   [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
   >>> list(enumerate(seasons, start=1))
   [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
   
   >>>for index, item in list(enumerate(seasons)):
   ...	#Do something
   ```

5. **filter(*function*, *iterable*)**

   Construct an iterator from those elements of *iterable* for which *function* returns true.

   If *function* is `None`, the identity function is assumed, that is, all elements of *iterable* that are false are removed.

   It is equivalent to the generator expression `(item for item initerable if function(item))` if function is not `None` and `(item for item in iterable if item)` if function is `None`.

6. **hash(*object*)**

   Return the hash value of the object (if it has one). They are used to quickly compare dictionary keys during a dictionary lookup.

   Numeric values that compare equal have the same hash value (even if they are of different types, as is the case for 1 and 1.0).

7. **map(*function*, *iterable*, *...*)**

   Return an iterator that applies *function* to every item of *iterable*, yielding the results.

   This is similar to *List Comprehensions*, when a single iterable is given.

8. **max(*iterable*, *[, *key*, *default*])**

   The argument `key=keyfunc` can be especially useful when used to find the *biggest* element according to a given function.

## To be continued