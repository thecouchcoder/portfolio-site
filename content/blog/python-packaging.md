+++
title = "A Beginner's Guide to Python Packaging"
date = 2024-08-18T12:03:47-05:00
+++

Once you've written Python code, you may be eager to share it for others to use. But how exactly do you share your code? The purpose of this guide is to cover the basic essentials to help you package up your Python code.

# Terminology

Let's begin with some terminology so we're on the same page. In Python, a module is a file containing definitions and statements - your .py files. A package is a directory that contains one or more modules. If you have a directory named Foo with a file named Bar.py, you have module Bar within package Foo. To import this you used dotted notation: `from Foo.Bar import {function name}`. This saves you from having collisions between module names in case some other package also has a module name Bar - imagine if there was `pandas.Bar`. To have Python treat a directory like a package you have to have an `__init__.py` file even if it's just a blank file.

# Developing a Package

Now let's write our first package - this will contain all the code we want to share plus the tools we need in order to share it. For reference, we will be building the following file structure.

```
library_example/
├── setup.py
├── greeter/
│ ├── __init__.py
│ └── greeter.py
```

First, create a directory called `library_example` followed by a directory called `greeter`. Within greeter create a file called `greeter.py`. The code we plan to share will be in `greeter.py`, put whatever you want in here. Mine looks like this:

```Python
def greet(name):
    print("Hello " + name + ". How are you today?")
```

Now we need to package our code. First, in greater adding a file named `__init__.py`. As mentioned earlier, this file will be completely empty, but Python needs this in order to know it's a directory. Next, within `library_example` create a file called `setup.py`. Python has a built in tool for packaging called `setuptools` which will be using. In `setup.py` add the following code.

```Python
from setuptools import setup, find_packages

setup(name='greeter', version='1.0', packages=find_packages(where=".", include=["greeter*"]))
```

This essentially setups all the metadata for your package. There are many more options, but for this basic example this is all we need.

# Packaging a Package

Python has 2 formats for packages - wheels and source distributions. We won't go into much detail here, but a wheel is a binary distribution of your code that can be downloaded and used in Python project, while source distributions involve packaging up your source code so the person who downloads it has to compile it into a wheel first in order to use. There's more on this in the challenges section.
Let's start by creating both a wheel and source distribution. Within `library_example` run

```
python setup.py sdist bdist_wheel
```

You can see a few new folders were created. The most important one is the `dist` folder which contains your source distribution and wheel.

# Testing Your Package Locally

When creating packages, you likely want to test them locally to make sure everything to working. This is very easy to do. On command line, within your virtual environment, navigate to the `library_example` directory and run `pip install .` Now if you run `pip list` you can see your package `greeter` is listed as being installed. Now in a completely different project you can create a `main.py` to test out your new package

```Python
from greeter.greeter import greet

if __name__ == '__main__':
    greet("Harry")
```

# Challenges

Here are some ideas for where to go from here:

- What other options could you specify in setup.py for setup tools?
- What is the difference between a wheel and source distribution is and why you would want to use one over the other?
- There are some current security concerns related to arbitrary code execution related to source distributions. What does this mean and how can you mitigate it? (Hint: research pyproject.toml and setup.cfg)
- How would you take your newly packaged code and upload it to a package index like PyPi?
- How does packaging code with Poetry differ from setuptools?
