# 0x03. Unittests and Integration Tests

## UnitTests

## Back-end

## Integration tests

![UNITESTS](https://private-user-images.githubusercontent.com/125453474/301067124-a4127bc3-bd13-4c42-81e3-94f71a2bcfbe.gif?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjE4OTAzMjQsIm5iZiI6MTcyMTg5MDAyNCwicGF0aCI6Ii8xMjU0NTM0NzQvMzAxMDY3MTI0LWE0MTI3YmMzLWJkMTMtNGM0Mi04MWUzLTk0ZjcxYTJiY2ZiZS5naWY_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNzI1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDcyNVQwNjQ3MDRaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0wNGI0YmMxNTNkNWNhZjIzZDkwOTk5OWJkNDNhZTViY2I1ZmI5ZDM3NjBhMDlmZmM2MjE4ZjkyNTVmNTFlMmVhJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.qOPS_YhANpNW3_BcmsherMwuvA2rJNU4rvSGqpHjAcY)

`Unit testing` is the process of testing that a particular function returns expected results for different set of inputs. A unit test is supposed to test standard inputs and corner cases. A unit test should only test the logic defined inside the tested function. Most calls to additional functions should be mocked, especially if they make network or database calls.

The goal of a unit test is to answer the question: if everything defined outside this function works as expected, does this function work as expected?

`Integration tests` aim to test a code path end-to-end. In general, only low level functions that make external calls such as HTTP requests, file I/O, database I/O, etc. are mocked.

Integration tests will test interactions between every part of your code.

Execute your tests with

    $ python -m unittest path/to/test_file.py

## Resources

__Read or watch:__

  - [unittest — Unit testing framework](https://docs.python.org/3/library/unittest.html)
  - [unittest.mock — mock object library](https://docs.python.org/3/library/unittest.mock.html)
  - [How to mock a readonly property with mock?](https://stackoverflow.com/questions/11836436/how-to-mock-a-readonly-property-with-mock)
  - [parameterized](https://pypi.org/project/parameterized/)
  - [Memoization](https://en.wikipedia.org/wiki/Memoization)

## Learning Objectives

At the end of this project, you are expected to be able to [explain to anyone](https://fs.blog/feynman-learning-technique/), __without the help of Google__:

  -The difference between unit and integration tests.
  - Common testing patterns such as mocking, parametrizations and fixtures

## Requirements

  - All your files will be interpreted/compiled on Ubuntu 18.04 LTS using `python3` (version 3.7)
  - All your files should end with a new line
  - The first line of all your files should be exactly `#!/usr/bin/env python3`
  - `A README.md` file, at the root of the folder of the project, is mandatory
  - Your code should use the `pycodestyle` style (version 2.5)
  - All your files must be executable
  - All your modules should have a documentation (`python3 -c 'print(__import__("my_module").__doc__)'`)
  - All your classes should have a documentation (`python3 -c 'print(__import__("my_module").MyClass.__doc__)'`)
  - All your functions (inside and outside a class) should have a documentation (`python3 -c 'print(__import__("my_module").my_function.__doc__)' and python3 -c 'print(__import__("my_module").MyClass.my_function.__doc__)'`)
  - A documentation is not a simple word, it’s a real sentence explaining what’s the purpose of the module, class or method (the length of it will be verified)
  - All your functions and coroutines must be type-annotated.

## Required Files

`utils.py` (or [download](https://intranet-projects-files.s3.amazonaws.com/webstack/utils.py))

<details>
<summary>Click to show/hide file contents</summary>

```
#!/usr/bin/env python3
"""Generic utilities for github org client.
"""
import requests
from functools import wraps
from typing import (
    Mapping,
    Sequence,
    Any,
    Dict,
    Callable,
)

__all__ = [
    "access_nested_map",
    "get_json",
    "memoize",
]


def access_nested_map(nested_map: Mapping, path: Sequence) -> Any:
    """Access nested map with key path.
    Parameters
    ----------
    nested_map: Mapping
        A nested map
    path: Sequence
        a sequence of key representing a path to the value
    Example
    -------
    >>> nested_map = {"a": {"b": {"c": 1}}}
    >>> access_nested_map(nested_map, ["a", "b", "c"])
    1
    """
    for key in path:
        if not isinstance(nested_map, Mapping):
            raise KeyError(key)
        nested_map = nested_map[key]

    return nested_map


def get_json(url: str) -> Dict:
    """Get JSON from remote URL.
    """
    response = requests.get(url)
    return response.json()


def memoize(fn: Callable) -> Callable:
    """Decorator to memoize a method.
    Example
    -------
    class MyClass:
        @memoize
        def a_method(self):
            print("a_method called")
            return 42
    >>> my_object = MyClass()
    >>> my_object.a_method
    a_method called
    42
    >>> my_object.a_method
    42
    """
    attr_name = "_{}".format(fn.__name__)

    @wraps(fn)
    def memoized(self):
        """"memoized wraps"""
        if not hasattr(self, attr_name):
            setattr(self, attr_name, fn(self))
        return getattr(self, attr_name)

    return property(memoized)
```

`client.py` (or download)


