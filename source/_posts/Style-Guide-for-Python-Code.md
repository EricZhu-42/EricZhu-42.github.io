---
title: Notes on "Style Guide for Python Code"
date: 2019-02-26
tags: [Python]
---
**This article is an abstract of *Style Guide for Python Code* by python-dev.**

To see the full document, click [HERE](https://www.python.org/dev/peps/pep-0008/)

## Basic Principles

> Code is read much more often than it is written. **Readability counts.**

The guidelines provided here are intended to improve the readability of code and make it consistent across the wide spectrum of Python code.

Consistency with this style guide is important. Consistency within a project is more important. **Consistency within one module or function is the most important.**

In particular: do not break backwards compatibility just to comply with this article!

## Code Lay-out

1. **Indentation**  

   Use 4 spaces per indentation level.  

   <!-- more -->

   Example:  

    ```python
    # Aligned with opening delimiter.
    foo = long_function_name(var_one, var_two,
                             var_three, var_four)
   
    # Add 4 spaces (an extra level of indentation) to distinguish arguments from the rest.
    def long_function_name(
            var_one, var_two, var_three,
            var_four):
        print(var_one)
   
    # Hanging indents should add a level.
    foo = long_function_name(
        var_one, var_two,
        var_three, var_four)
    ```

2. **Tabs or Spaces?**  

   **Spaces are the preferred indentation method.**

   Tabs should be used solely to remain consistent with code that is already indented with tabs.

3. **Maximum Line Length**

   Limit all lines to a maximum of **79 characters**.

   For flowing long blocks of text with fewer structural restrictions (docstrings or comments), the line length should be limited to **72 characters**.

   The preferred way of wrapping long lines is by using Python's implied **line continuation inside parentheses, brackets and braces**. 

   **Backslashes** may still be appropriate at times. For example, long, multiple `with`-statements cannot use implicit continuation, so backslashes are acceptable.

4. **Should a Line Break Before or After a Binary Operator?**  

   Although formulas within a paragraph always break after binary operations and relations, displayed formulas always break before binary operations.

   ```python
   # Easy to match operators with operands
   income = (gross_wages
             + taxable_interest
             + (dividends - qualified_dividends)
             - ira_deduction
             - student_loan_interest)
   ```

5. **Blank Lines**    

     Surround **top-level function and class definitions** with **two** blank lines.

     **Method definitions inside a class** are surrounded by **a single** blank line.

     Use blank lines in functions, sparingly, to **indicate logical sections**.

6. **Source File Encoding**

   Code in the core Python distribution should always use **UTF-8** (or ASCII in Python 2).  

7. **Imports**  

   Imports should usually be on separate lines:  

    ```python
    import os
    import sys
    ```

   It's okay to say this though:

    ```python
    from subprocess import Popen, PIPE
    ```
    Imports should be grouped in **the following order**:

   1. Standard library imports.
   2. Related third party imports.
   3. Local application/library specific imports.

    You should **put a blank line between each group of imports**.

    Absolute imports are recommended, as they are usually more readable and tend to be better behaved.

    ```python
    from mypkg.sibling import example
    ```

    Wildcard imports (from \<module> import *) should be **avoided**.

8. **Module Level Dunder Names**  
   Module level "dunders" (i.e. names with two leading and two trailing underscores) such as \_\_all\_\_, \_\_author\_\_, \_\_version\_\_, etc. should be placed after the module docstring but before any import statements except from \_\_future\_\_ imports. Python mandates that future-imports must appear in the module before any other code except docstrings:

   ```python
   """This is the example module.
   
   This module does stuff.
   """
   
   from __future__ import barry_as_FLUFL
   
   __all__ = ['a', 'b', 'c']
   __version__ = '0.1'
   __author__ = 'Cardinal Biggles'
   
   import os
   import sys
   ```

## String Quotes

   Single-quoted strings and double-quoted strings are all acceptable.

   **For triple-quoted strings, always use double quote characters.**

## Whitespace in Expressions and Statements

​	Avoid extraneous whitespace in the following situations:

- Immediately inside parentheses, brackets or braces.

   ```python
   Yes: spam(ham[1], {eggs: 2})
   No:  spam( ham[ 1 ], { eggs: 2 } )
   ```

- Between a trailing comma and a following close parenthesis.

   ```python
   Yes: foo = (0,)
   No:  bar = (0, )
   ```

- Immediately before a comma, semicolon, or colon:

  ```python
  Yes: if x == 4: print x, y; x, y = y, x
  No:  if x == 4 : print x , y ; x , y = y , x
  ```
- However, in a slice the colon acts like a binary operator, and should have equal amounts on either side (treating it as the operator with the lowest priority). In an extended slice, both colons must have the same amount of spacing applied. **Exception: when a slice parameter is omitted, the space is omitted.**

  ```python
  # Yes:
  ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
  ham[lower:upper], ham[lower:upper:], ham[lower::step]
  ham[lower+offset : upper+offset]
  ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
  ham[lower + offset : upper + offset]
  
  # No:
  ham[lower + offset:upper + offset]
  ham[1: 9], ham[1 :9], ham[1:9 :3]
  ham[lower : : upper]
  ham[ : upper]
  ```

- Immediately before the open parenthesis that starts the argument list of a function call:

  ```python
  Yes: spam(1)
  No:  spam (1)
  ```

- Immediately before the open parenthesis that starts an indexing or slicing:

  ```python
  Yes: dct['key'] = lst[index]
  No:  dct ['key'] = lst [index]
  ```

- More than one space around an assignment (or other) operator to align it with another.

  ```python
  # Yes:
  x = 1
  y = 2
  long_variable = 3
  
  # No:
  x             = 1
  y             = 2
  long_variable = 3
  ```

- Avoid trailing whitespace anywhere. Because it's usually invisible, it can be confusing.

- Always surround **these** binary operators with a single space on either side: 
  - assignment (=), augmented assignment (+=, -=, etc.)   
  - comparisons (==, <, >, !=, <>, <=, >=, in, not in, is, is not)   	
  - Booleans (and, or, not)

- If operators with different priorities are used, **consider adding whitespace around the operators with the lowest priority(ies).**   
	However, never use more than one space, and always have the same amount of whitespace on both sides of a binary operator.

	```python
	#Yes:
	i = i + 1
	submitted += 1
	x = x*2 - 1
	hypot2 = x*x + y*y
	c = (a+b) * (a-b)
	```

- **Compound statements (multiple statements on the same line) are generally discouraged.**
- While sometimes it's okay to put an if/for/while with a small body on the same line, never do this for multi-clause statements.

## When to Use Trailing Commas

**When a list of values, arguments or imported items is expected to be extended over time, always adding a trailing comma, and add the close parenthesis/bracket/brace on the next line.**

```python
FILES = [
    'setup.cfg',
    'tox.ini',
    ]
initialize(FILES,
       error=True,
       )
```

## Comments

Comments that contradict the code are worse than no comments. Always make a priority of keeping the comments up-to-date when the code changes!

Comments should be complete sentences. The first word should be capitalized, unless it is an identifier that begins with a lower case letter (**never alter the case of identifiers!**).

1. **Block Comments**

   **Each line of a block comment starts with a `#` and a single space** (unless it is indented text inside the comment).

2. **Inline Comments**

   **Use inline comments sparingly.**

   Inline comments are unnecessary and in fact distracting if they state the obvious.

3. **Documentation Strings**

   Write docstrings for **all** public modules, functions, classes, and methods. 

   Docstrings are not necessary for non-public methods, but you should have a comment that describes what the method does. **This comment should appear after the `def` line.**

   Note that most importantly, the `"""` that ends a multiline docstring should be on a line by itself:

    ```python
    """Return a foobang
   
    Optional plotz says to frobnicate the bizbaz first.
    """
    ```

   For one liner docstrings, please keep the closing `"""` on the same line.

## Naming Conventions

1. **Overriding Principle**

   Names that are visible to the user as public parts of the API should follow conventions that reflect usage rather than implementation.

2. **Descriptive: Naming Styles**

   The following special forms using leading or trailing underscores are recognized.

   - `_single_leading_underscore`: **weak "internal use" indicator**.  
     E.g. `from M import *` does not import objects whose name starts with an underscore.

   - `single_trailing_underscore_`: used by convention to **avoid conflicts with Python keyword**.
   - `__double_leading_underscore`: when naming **a class attribute (not intended for subclass to use)**, invokes name mangling (inside class FooBar, `__boo` becomes`_FooBar__boo`) 
   - `__double_leading_and_trailing_underscore__`: "magic" objects or attributes that live in user-controlled namespaces. E.g. `__init__`, `__import__` or `__file__`. **Never invent such names**; only use them as documented.

3. **Prescriptive: Naming Conventions**

   1. **Names to Avoid**

      Never use the characters 'l' (lowercase letter el), 'O' (uppercase letter oh), or 'I' (uppercase letter eye) as single character variable names.

   2. **ASCII Compatibility**

      Identifiers used in the standard library must be ASCII compatible

   3. **Package and Module Names**

      **Python packages should have short, all-lowercase names, although the use of underscores is discouraged.**

      When an extension module written in C or C++ has an accompanying Python module that provides a higher level (e.g. more object oriented) interface, the C/C++ module has a leading underscore (e.g. `_socket`).

   4. **Class Names**

      **Class names should normally use the CapWords convention.**

   5. **Exception Names**

      Because exceptions should be classes, the class naming convention applies here.  
      However, **you should use the suffix "Error" on your exception names** (if the exception actually is an error).

   6. **Global Variable Names**

      The conventions are about the same as those for functions.

      Modules that are designed for use via `from M import *` should **use the `__all__` mechanism** to prevent exporting globals.

   7. **Function and Variable Names**

      **Function names should be lowercase, with words separated by underscores as necessary to improve readability.**

      Variable names follow the same convention as function names.

   8. **Function and Method Arguments**

      **Always** use `self` for the first argument to instance methods.

      **Always** use `cls` for the first argument to class methods.

      If a function argument's name clashes with a reserved keyword, it is generally better to **append a single trailing underscore** rather than use an abbreviation or spelling corruption. Thus `class_` is better than `clss`. (Perhaps better is to avoid such clashes by using a synonym.)

   9. **Method Names and Instance Variables**

      Use the function naming rules: **lowercase with words separated by underscores** as necessary to improve readability.

      Use one leading underscore only for non-public methods and instance variables.

   10. **Constants**

       Constants are usually defined on a module level and written in **all capital letters with underscores separating words**. Examples include `MAX_OVERFLOW` and `TOTAL`.

   11. **Designing for Inheritance**

       Always decide whether a class's methods and instance variables  should be public or non-public. **If in doubt, choose non-public**; it's easier to make it public later than to make a public attribute non-public.

        - **Public attributes should have no leading underscores.**
        - If your public attribute name collides with a reserved keyword, append a single trailing underscore to your attribute name.
        - If your class is intended to be subclassed, and you have attributes that you do not want subclasses to use, consider naming them with **double leading underscores and no trailing underscores**.

4. **Public and Internal Interfaces**

   **Documented interfaces are considered public**, unless the documentation explicitly declares them to be provisional or internal interfaces exempt from the usual backwards compatibility guarantees. **All undocumented interfaces should be assumed to be internal**.

   To better support introspection, **modules should explicitly declare the names in their public API using the `__all__` attribute.** Setting `__all__` to an empty list indicates that the module has no public API.

   Even with `__all__` set appropriately, internal interfaces (packages, modules, classes, functions, attributes or other names) should still **be prefixed with a single leading underscore**.

## Programming Recommendations

- Code should be written in a way that does not disadvantage other implementations of Python.

- **Comparisons to singletons like None should always be done with `is` or `is not`, never the equality operators.**

  Also, beware of writing `if x` when you really mean `if x is not None` (i.e. an empty list will be considered as False but not None).

- Use `is not` operator rather than `not ... is`. 

- Always use a def statement instead of an assignment statement that binds a lambda expression directly to an identifier.

   ```python
   #Yes:
   def f(x): return 2*x
   
   #No:
   f = lambda x: 2*x
   ```

- **Derive exceptions from `Exception` rather than `BaseException`.** Direct inheritance from `BaseException` is reserved for exceptions where catching them is almost always the wrong thing to do.

- **When catching exceptions, mention specific exceptions whenever possible** instead of using a bare `except`.

- For all try/except clauses, limit the `try` clause to the **absolute minimum amount of code** necessary.

- Be consistent in return statements.If any return statement returns an expression, any return statements where **no value is returned should explicitly state this as `return None**`, and an explicit return statement should be present at the end of the function (if reachable).

- Use string methods instead of the string module.

- Use `''.startswith()` and `''.endswith()` instead of string slicing to check for prefixes or suffixes.

- Object type comparisons should always use **isinstance() instead of comparing types** directly.

- For sequences, (strings, lists, tuples), use the fact that empty sequences are false.

- **Don't compare boolean values to True or False** using `==`.

  ```python
  Yes:   if greeting:
  No:    if greeting == True:
  Worse: if greeting is True:
  ```