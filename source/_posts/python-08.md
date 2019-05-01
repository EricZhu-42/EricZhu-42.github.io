---
title: Notes on "Python Cookbook"
date: 2019-03-21
tags: [python]
---

## Data Structure and Algorism

1. Unpacking assignment can be applied to any iterable object.

2. Use `*temp` to unpack values of an uncertain number.
    Use `_` to declare a variable that you want to discard.

   ```python
   def drop_first_last(grades):
       first, *middle, last = grades	#*middle is a List
       return avg(middle)
   ```

3. Use `heapq.nlargest()` and `heapq.nsmallest()` to get the largest/smallest n-elements in a container.

	<!-- more --> 

4. Use  `collections.defaultdict` to construct a mulidict(Dictionary where a key corresponds to multiple values)

5. Use `collections.OrderedDict` to construct a ordered dictionary.

   (It's a chain table and will cost double memory compared with normal dictionary)

6.  If you need to get the maximum value of a dictionary, use `zip` to reverse key & value. `max()` will compare the first element by default.

    (This is more useful since the return value of `max()` will be a tuple containing key & value)

7. The type of `dict.keys()` is `dict_keys`, which supports operations for Sets like AND/OR by default.

8. `slice()` creates a slice object, which can be used in any circumstances when a slice is needed. This will improve the readability of your code.
  ```python
  SHARES = slice(20, 23) 
  PRICE = slice(31, 37) 
  cost = int(record[SHARES]) * float(record[PRICE])
  ```

9. `collections.Counter` and `collections.Counter.most_common(n)` are significantly useful when you want to count something.

## String and Text

1. Use `re.split()` to split a string in complex circumstances.
	```python 
    >>> line = 'asdf fjdk; afed, fjek,asdf, foo' 
    >>> import re 
    >>>  re.split(r'[;,\s]\s*', line) 
    ['asdf', 'fjdk', 'afed', 'fjek', 'asdf', 'foo']
   ```

