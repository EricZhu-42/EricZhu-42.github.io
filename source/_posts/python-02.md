---
title: Python-Brief Introduction of Grammar 
date: 2019-02-26
tags: [python]
---

​	This article gives a brief introduction of grammar of Python

### Basic Grammar

1. **Definition & assignment of variables**
```python
   foo = 1
   bar, baz = "Hello world!", True
```
2. **Condition statements**
```python
   if a < b:
   ​	print ("YES")
   elif a > b:
   ​	print ("NO")
   else:
   ​	pass	# Do nothing
```
<!-- more -->

### Basic containers 

1. **List**
```python
   names = ["Dave", "Mike", "Adams"]
   names.append("Eric")
   names.insert(2, "Jobs")
   sub_a = names[0:2]
   sub_b = names[0:-1]
   reverse_str = names[::-1]
   name[0:2] = ["Dave", "Mark", "Jeff"]
   a = [1, 2, 3] + [4, 5]
   b = [1, True, "Eric", [2, 3]]
```
2. **Tuple**
```python
   stock = ("Good", 100, True)
   items = ("Apple", )	# ',' is necessary since ( ) are always valid
```
   Items in a tuple **CANNOT** be changed. But if it is another container, things can be different.
```python
   a = ["Apple"]
   b = (a, )
   a.append("Pear")	# Valid
```
   Tuple takes less memory than list, so use them if possible.

3. **Set**

   Sets are used to contain a series of **unique** elements **without order**.
```python
   s = set([1, 2, 3, 4])
   a = t | s	# Union of t and s
   b = t | s	# Intersection of t and s
   c = t - s	
   d = t ^ s	# Symmetric difference of t and s
   t.add('x') # Add an item
   s.update([1, 2, 3]) # Add multiple items
   t.remove('x') # Remove an item
```

4. **Dictionary** 

   Dictionary contains some key-value pairs.
```python
	stock = {
        "name"   : "Eric",
        "Age"    : 19,
        "Gender" : "Male"
	}
	stock["Balance"] = 0
	p = stock.get("Age", 0)
```

### Iterate and loop  
```python
	For item in range(0,5):
		print (item, end='')	# 0 1 2 3 4
	with open(data.txt) as f:
		for line in f:
			print (line)	# Print all the lines in this file.
```

### Function

​	Use `def` to define functions.
```python
	def new_max(a,b):
		if a > b:
			return a
		else:
    		return b
```

​	If you want to return multiple variables, return them as a tuple.
```python
	def my_func(a,b=100):
		return (a,b)
    x = eval(input("Please input a number"))
    y = my_func(x,0)
```

​	use `global` to get global variables.
```python
	count = 0
	def my_func(x):
		global count
		count += 1
		return count
```

### Class and object

​	**All values in Python are OBJECTS.** We can design your own class easily.
```python
Class Stack(object):	# Stack inherit from object
	def __init__(self):		# Initialize this stack
        self.stack = [] 
    def push(self,object):
        self.stack.append(object)
    def pop(self):
        return self.stack.pop()
s = Stack()
s.push("David")
del s
```

### Error

​	Error will lead to the termination of program. But we can use `try` and `except` to catch and deal with them.
```python
	try:
		f = open("test.txt","r")
	except IOError as e:	# Catch IOError
		print (e)
	except:		# Catch other errors
		print("GG")
```

​	We can use `raise` to cause an error manually.

```python
	raise RuntimeError("Computer says no")
```

**Modules**

​	We can divide a program as several modules for easy maintenance.
```python
	#file : temp.py
	def my_max(a,b):
		return max(a,b)
	if __name__ == '__main__':
		print("Hello")
	
	#file : main.py
	import temp as t	# No output here
	x = t.my_max(10,15)		# x == 15
```

### References

​	1. David M. Beazley. *Python Essential Reference*. Sams, 2001.