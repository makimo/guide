## Style Guide for Python Code

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

#### 4. String interpolation

Do not use `printf`-style string formatting in Python 3 projects
(also known as "old-style" or `%`-formatting), as it is discouraged
in the [python docs](https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting).
Prefer [formatted string literals](https://docs.python.org/3/reference/lexical_analysis.html#formatted-string-literals)
(for python 3.6+) or [`str.format(*args, **kwargs)`](https://docs.python.org/3.6/library/stdtypes.html#str.format)
for Python 3.5 and below. If user-provided content is being formatted,
please consider using [Template strings](https://docs.python.org/3/library/string.html#template-strings).

Examples:

```python
# Bad
my_string = 'My embedded string is: %s' % my_embedded_string

# OK, if on Python 3.5 and below
my_string = 'My embedded string is: {}'.format() 

# OK
my_string = f'My embedded string is: {my_embedded_string}'

# If user_string_template was provided by the user
user_string_template = 'My embedded string is: $embedded'
my_string_template = Template(user_string_template).safe_substitute(
    embedded=my_embedded_string
)
```

#### 5. String-breaking

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

#### 6. Dyadic operators

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

#### 7. Blank lines

As a rule of thumb, readability is preferable over smaller line count.
It is, therefore, desirable to write code with room to breathe.
Blank lines are used to that effect. The following section describes
_necessary_ and _sufficient_ rules for inserting them.

##### 7.1. Two blank lines between top-level objects (classes, functions, variables).

```python
def foo(): pass


class Foo:
    pass


class Bar:
    pass
```

##### 7.2. Module-level variables should be separated from imports with one blank line.

```python
import sys
import argparse

SOME_MODULE_VARIABLE = 'string'
```

##### 7.3. Methods within class declarations are separated with one blank line.

```python
class Foo:
    def foo(self):
        pass

    def bar(self):
        pass
```

##### 7.4. No lines between begginning of the class declaration and first method/variable.

```python
def class Foo:
    some_variable = null


def class Bar:
    def foo(self):
        pass
```

##### 7.5. No blank lines between docstring and first next statement with the exception of class docstring if no class variables are declared.

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

##### 7.6. If dunder variables are defined, separated them with one blank line.

```python
__version__ = '1.6.7'
__all__ = ('Foo, 'Bar')

import sys
```

##### 7.7. One blank line between each import section (stdlib, 3rd-party, local).

```python
import sys
import asyncio

from django import something

from .local_file import LocalClass
```

##### 7.8. One blank line before and after statements that open a new block (e.g. if-statement), with exception of statements at the first line of their block

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

##### 7.9. No blank lines between docstring and first statement in method/function

```python
def foo():
    """
    <docstring>
    """
    first = 'statement'
```

##### 7.10. Two blank lines after import section (or module variable)

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

#### 8. Imports

Sort imports using `isort`.

#### 9. Docstrings and Comments

Docstring conventions used at Makimo are mostly based on the three following
documents (which we recommend to read):

1. [PEP 257 - Docstring Conventions](https://www.python.org/dev/peps/pep-0257/)
   (Accepted, necessary rules)
2. [PEP 224 - Attribute Docstrings](https://www.python.org/dev/peps/pep-0224/)
   (Officially rejected, but used as a convention anyway)
3. [Google Python Style Guide, 3.8 Comments ands Docstrings](
https://github.com/google/styleguide/blob/gh-pages/pyguide.md#38-comments-and-docstrings)
4. [Example Google Style Python Docstrings](
https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)

The rule of thumb is to use docstrings whenever documenting public interface.
Comments should be left to such cases as laying out a structure and inner-workings
of the code with the expectation that they help the programmer modifying this
particular file or module.

Remember to use only double-quotes (`"`). This is for consistency mostly.

#### 10. Google Style Example

```python
# -*- coding: utf-8 -*-
"""Example Google style docstrings.

This module demonstrates documentation as specified by the `Google Python
Style Guide`_. Docstrings may extend over multiple lines. Sections are created
with a section header and a colon followed by a block of indented text.

Example:
    Examples can be given using either the ``Example`` or ``Examples``
    sections. Sections support any reStructuredText formatting, including
    literal blocks::

        $ python example_google.py

Section breaks are created by resuming unindented text. Section breaks
are also implicitly created anytime a new section starts.

Attributes:
    module_level_variable1 (int): Module level variables may be documented in
        either the ``Attributes`` section of the module docstring, or in an
        inline docstring immediately following the variable.

        Either form is acceptable, but the two should not be mixed. Choose
        one convention to document module level variables and be consistent
        with it.

Todo:
    * For module TODOs
    * You have to also use ``sphinx.ext.todo`` extension

.. _Google Python Style Guide:
   http://google.github.io/styleguide/pyguide.html

"""

module_level_variable1 = 12345

module_level_variable2 = 98765
"""int: Module level variable documented inline.

The docstring may span multiple lines. The type may optionally be specified
on the first line, separated by a colon.
"""


def function_with_types_in_docstring(param1, param2):
    """Example function with types documented in the docstring.

    `PEP 484`_ type annotations are supported. If attribute, parameter, and
    return types are annotated according to `PEP 484`_, they do not need to be
    included in the docstring:

    Args:
        param1 (int): The first parameter.
        param2 (str): The second parameter.

    Returns:
        bool: The return value. True for success, False otherwise.

    .. _PEP 484:
        https://www.python.org/dev/peps/pep-0484/

    """


def function_with_pep484_type_annotations(param1: int, param2: str) -> bool:
    """Example function with PEP 484 type annotations.

    Args:
        param1: The first parameter.
        param2: The second parameter.

    Returns:
        The return value. True for success, False otherwise.

    """


def module_level_function(param1, param2=None, *args, **kwargs):
    """This is an example of a module level function.

    Function parameters should be documented in the ``Args`` section. The name
    of each parameter is required. The type and description of each parameter
    is optional, but should be included if not obvious.

    If \*args or \*\*kwargs are accepted,
    they should be listed as ``*args`` and ``**kwargs``.

    The format for a parameter is::

        name (type): description
            The description may span multiple lines. Following
            lines should be indented. The "(type)" is optional.

            Multiple paragraphs are supported in parameter
            descriptions.

    Args:
        param1 (int): The first parameter.
        param2 (:obj:`str`, optional): The second parameter. Defaults to None.
            Second line of description should be indented.
        *args: Variable length argument list.
        **kwargs: Arbitrary keyword arguments.

    Returns:
        bool: True if successful, False otherwise.

        The return type is optional and may be specified at the beginning of
        the ``Returns`` section followed by a colon.

        The ``Returns`` section may span multiple lines and paragraphs.
        Following lines should be indented to match the first line.

        The ``Returns`` section supports any reStructuredText formatting,
        including literal blocks::

            {
                'param1': param1,
                'param2': param2
            }

    Raises:
        AttributeError: The ``Raises`` section is a list of all exceptions
            that are relevant to the interface.
        ValueError: If `param2` is equal to `param1`.

    """
    if param1 == param2:
        raise ValueError('param1 may not be equal to param2')
    return True


def example_generator(n):
    """Generators have a ``Yields`` section instead of a ``Returns`` section.

    Args:
        n (int): The upper limit of the range to generate, from 0 to `n` - 1.

    Yields:
        int: The next number in the range of 0 to `n` - 1.

    Examples:
        Examples should be written in doctest format, and should illustrate how
        to use the function.

        >>> print([i for i in example_generator(4)])
        [0, 1, 2, 3]

    """
    for i in range(n):
        yield i


class ExampleError(Exception):
    """Exceptions are documented in the same way as classes.

    The __init__ method may be documented in either the class level
    docstring, or as a docstring on the __init__ method itself.

    Either form is acceptable, but the two should not be mixed. Choose one
    convention to document the __init__ method and be consistent with it.

    Note:
        Do not include the `self` parameter in the ``Args`` section.

    Args:
        msg (str): Human readable string describing the exception.
        code (:obj:`int`, optional): Error code.

    Attributes:
        msg (str): Human readable string describing the exception.
        code (int): Exception error code.

    """

    def __init__(self, msg, code):
        self.msg = msg
        self.code = code


class ExampleClass(object):
    """The summary line for a class docstring should fit on one line.

    If the class has public attributes, they may be documented here
    in an ``Attributes`` section and follow the same formatting as a
    function's ``Args`` section. Alternatively, attributes may be documented
    inline with the attribute's declaration (see __init__ method below).

    Properties created with the ``@property`` decorator should be documented
    in the property's getter method.

    Attributes:
        attr1 (str): Description of `attr1`.
        attr2 (:obj:`int`, optional): Description of `attr2`.

    """

    def __init__(self, param1, param2, param3):
        """Example of docstring on the __init__ method.

        The __init__ method may be documented in either the class level
        docstring, or as a docstring on the __init__ method itself.

        Either form is acceptable, but the two should not be mixed. Choose one
        convention to document the __init__ method and be consistent with it.

        Note:
            Do not include the `self` parameter in the ``Args`` section.

        Args:
            param1 (str): Description of `param1`.
            param2 (:obj:`int`, optional): Description of `param2`. Multiple
                lines are supported.
            param3 (:obj:`list` of :obj:`str`): Description of `param3`.

        """
        self.attr1 = param1
        self.attr2 = param2
        self.attr3 = param3  #: Doc comment *inline* with attribute

        #: list of str: Doc comment *before* attribute, with type specified
        self.attr4 = ['attr4']

        self.attr5 = None
        """str: Docstring *after* attribute, with type specified."""

    @property
    def readonly_property(self):
        """str: Properties should be documented in their getter method."""
        return 'readonly_property'

    @property
    def readwrite_property(self):
        """:obj:`list` of :obj:`str`: Properties with both a getter and setter
        should only be documented in their getter method.

        If the setter method contains notable behavior, it should be
        mentioned here.
        """
        return ['readwrite_property']

    @readwrite_property.setter
    def readwrite_property(self, value):
        value

    def example_method(self, param1, param2):
        """Class methods are similar to regular functions.

        Note:
            Do not include the `self` parameter in the ``Args`` section.

        Args:
            param1: The first parameter.
            param2: The second parameter.

        Returns:
            True if successful, False otherwise.

        """
        return True

    def __special__(self):
        """By default special members with docstrings are not included.

        Special members are any methods or attributes that start with and
        end with a double underscore. Any special member with a docstring
        will be included in the output, if
        ``napoleon_include_special_with_doc`` is set to True.

        This behavior can be enabled by changing the following setting in
        Sphinx's conf.py::

            napoleon_include_special_with_doc = True

        """
        pass

    def __special_without_docstring__(self):
        pass

    def _private(self):
        """By default private members are not included.

        Private members are any methods or attributes that start with an
        underscore and are *not* special. By default they are not included
        in the output.

        This behavior can be changed such that private members *are* included
        by changing the following setting in Sphinx's conf.py::

            napoleon_include_private_with_doc = True

        """
        pass

    def _private_without_docstring(self):
        pass
```
