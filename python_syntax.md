### Style Guide for Python Code

### Abstract

This document gives coding conventions for the Python code. It is concerned
with syntax-level constructs. For Django-specific advice refer to [...].

It will come with no surprise that a good starting point for any Python
style-guide is <a href="https://www.python.org/dev/peps/pep-0008/">PEP-8</a>.
In case of any uncertainties, and regarding topics not covered in this guide,
it is advised to consult it first. As authors put it:

> The guidelines provided [in PEP-8] are intended to improve the readability
of code and make it consistent across the wide spectrum of Python code. As
PEP 20 says, "Readability counts". A style guide is about consistency.
Consistency with this style guide is important. Consistency within a project
is more important. Consistency within one module or function is the most
important. However, know when to be inconsistent -- sometimes style guide
recommendations just aren't applicable. When in doubt, use your best judgment.
Look at other examples and decide what looks best. And don't hesitate to ask!

Therefore, PEP-8, or this document, should not be treated as overriding
authority ("foolish consistency is the hobgoblin
of little minds"). Whenever unsure which rule takes precendence, consult the
following hierarchy (from the most important):

1. Whatever makes the most sense in the context of specific syntactic
construct, i.e. your own judgement.
2. Module/function/class/block-specific conventions.
3. Project-specific conventions.
4. This guide.
5. PEP-8.

Finally, orthogonal to this list is the judgement of whoever is reviewing your
code. Follow suggestions of the reviewer and/or be prepared to explain and
motivate your decisions.

### Content

#### 1. Indentation

4-spaces. No tabs.

#### 2. Line length

Try to fit 70–80 character if possible. If readability is at risk, aim for
90/100. Generally, if you're going over 100 you're doing something wrong.
Consider configuring your text-editor to mark columns 70, 80 and 90.

#### 3. Function invocation

Prefer this wherever possible:

```python
some_function_name(
    some_argument,                # This way I can easily add comment here.
    some_other_argument,          # On this line too.
    yet_another_one,
    something_optional=some_value # And here.
)
```
If you need add comments but the line is too narrow, you can format it as
follows:

```python
some_function_name(
    # Lorem ipsum dolor sit amet, consectetur adipiscing
    # elit, sed do eiusmod tempor incididunt ut labore et
    # dolore magna aliqua.
    some_long_argument_name,
    # Ut enim ad minim veniam, quis
    # nostrud exercitation ullamco laboris nisi ut aliquip
    # ex ea commodo consequat. Duis aute
    some_even_longer_argument_name,
    yet_another_one,
    something_optional=some_value
)
```

You can group arguments with short names like so:

```python
some_function_name(
    'a', 'b', 'c',
    'd', 'e', 'f',
)
```

This is acceptable too:

```python
some_function_name(
    some_argument, some_other_argument, yet_another_one)
```

Or this:

```python
some_function_name(some_argument,
                   some_other_argument, yet_another_one)
```

#### 4. String-breaking

These:

```python
some_variable = ('Lorem ipsum dolor sit amet, consectetur adipiscing elit. '
                 'Sed ac sapien eu neque vehicula rhoncus.')

some_variable = (
    'Lorem ipsum dolor sit amet, consectetur adipiscing elit. '
    'Sed ac sapien eu neque vehicula rhoncus.'
)
```
...are prettier than these:

```python
some_variable = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit.' \
                'Sed ac sapien eu neque vehicula rhoncus.'

some_variable = '''\
Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Sed ac sapien eu neque vehicula rhoncus.\
'''
```

Note that examples above play nicely with `gettext`.

#### 5. Dyadic operators

It is preferrable to:

1. Chain operators using brackets (`()`) rather than `\`.
2. Put operators in front of continuing lines instead of at the end.

```python
income = (
    gross_wages
    + taxable_interest
    + (dividends - qualified_dividends)
    - ira_deduction
    - student_loan_interest
)
```

Alternatively,
```python
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

The same rules apply to boolean expressions:

```python
truth_value = (
    some_boolean_variable_name
    and some_long_boolean_variable_name
    or some_other_var
    and not yet_another_one
)
```

#### 6. Blank lines

As a rule of thumb, readability is preferable over smaller line count.
It is, therefore, desirable to write code with room to breathe.
Blank lines are used to that effect. The following section describes
_necessary_ and _sufficient_ rules for inserting them.

##### 6.1. Two blank lines between top-level objects (classes, functions, variables).

```python
def foo(): pass


class Foo:
    pass


class Bar:
    pass
```

##### 6.2. Module-level variables should be separated from imports with one blank line.

```python
import sys
import argparse

SOME_MODULE_VARIABLE = 'string'
```

##### 6.3. Methods within class declarations are separated with one blank line.

```python
class Foo:
    def foo(self):
        pass

    def bar(self):
        pass
```

##### 6.4. No lines between begginning of the class declaration and first method/variable.

```python
def class Foo:
    some_variable = null

def class Bar:
    def foo(self):
        pass
```

##### 6.5. No blank lines between docstring and first next statement with the exception of class docstring if no class variables are declared.

```python
def class Foo:
    """
    <docstring>
    """
    some_variable = null

def class Bar:
    """
    <docstring>
    """

    def foo(self):
        pass
```

##### 6.6. If dunder variables are defined, separated them with one blank line.

```python
__version__ = '1.6.7'
__all__ = ('Foo, 'Bar')

import sys
```

##### 6.7. One blank line between each import section (stdlib, 3rd-party, local).

```python
import sys
import asyncio

from django import something

from .local_file import LocalClass
```

##### 6.8. One blank line before and after statements that open a new block (e.g. if-statement), with exception of statements at the first line of their block

```python
if a:
    return True

b = x or None

if b is not None:
    do_work()
else:
    def foo(): # No blank here
        pass

return False
```

If writing an if-statement with multiple else-branches and repetetive code, you
*can* throw in an extra line before each branch:

```python
if something0:
    a = 1 + 51 + 71 + 545 + 963 + 4
    b = 5 + 87 + 64 + 91 + 35 + 667
    c = 25 + 64 + 8 + 23 + 947 + 37

elif something1:
    a = 1 + 51 + 71 + 545 + 963 + 4
    b = 5 + 87 + 64 + 91 + 35 + 667
    c = 25 + 64 + 8 + 23 + 947 + 37

elif something2:
    a = 1 + 51 + 71 + 545 + 963 + 4
    b = 5 + 87 + 64 + 91 + 35 + 667
    c = 25 + 64 + 8 + 23 + 947 + 37
```

##### 6.9. No blank lines between docstring and first statement in method/function

```python
def foo():
    """
    <docstring>
    """
    first = 'statement'
```

##### 6.10. Two blank lines after import section (or module variable)

```python
import sys


class Foo:
   pass
```

---
Putting it all together:

```python
"""
Module docstring
"""
__version__ = '1.6.7'
__all__ = ('Foo', 'Bar')

import sys
import asyncio

MODULE_VARIABLE = 777


class Foo:
    """
    Docstring
    """

    def __init__(self):
        pass

    def method(self):
        pass


class Bar:
    """
    Docstring
    """
    class_variable = '1'


def foo():
    pass
```

### 7. Imports

Sort imports using `isort`.
