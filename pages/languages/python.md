---
permalink: /languages/python/
title: Python
parent: Languages
---

# Python Ecosystem Guide

This guide explains the basics of setting up Python and related helpful tools,
so that you can run and modify a Python-based project on your own computer. We
wrote this for people inside and outside 18F who want to run and contribute to
projects we build, and who aren't already familiar with working on Python
projects. This guide is both for people who write a lot of code and for people
who don't write any code (setting up this development environment doesn't
require coding knowledge).

# Installing Python

If you use OS X or some Linux distributions (such as Ubuntu), your system
already has Python installed. The downside is that it is probably an
out-of-date version and it is installed in a system-level directory which you
should **not** modify, as the host Operating System may rely on it for some of
its functionality.

You can ignore that default version and instead download and install Python
yourself in a user-controlled directory that you can modify at will.  There
are several ways you can download and install Python:

- Directly from the Python web site:  https://www.python.org/downloads/
- On a Linux system, with the OS package manager (though this may not
  provide you with an entirely up-to-date version).
- On a recent OS X system, with [Homebrew](http://brew.sh/).
- With a Python version manager:
  - [pyenv](https://github.com/yyuu/pyenv)
  - [Conda](http://conda.pydata.org/docs/)
  - [Python Launcher for Windows(as of version 3.3)](https://docs.python.org/3/using/windows.html#python-launcher-for-windows)

When you install Python directly on a UNIX-based system, Python 2.x
installations use the `python` and `pip` command line shortcuts and
Python 3.x installations use the `python3` and `pip3` shortcuts. Both
versions will play nicely side-by-side with each other, unlike a lot of other
dynamic languages (e.g., Ruby, Perl).  Windows-based environments are slightly
different yet can still accommodate both versions installed directly.
However, if you install Python via this method or an OS-based package manager,
it may be installed in a system-managed area and therefore require
administrator privileges to modify.  This includes installing third-party
packages.  If you choose this route, use the command line shortcuts that match
the version you want to use.

If you use a version manager for installing Python, this buys you the ability
to install and manage multiple versions of the language easily.  This is
especially useful should you need to work on projects that have hard
dependencies on the language version, especially given the Python community
split between the 2.x and 3.x series (at 18F we recommend the latest 3.x
release for all new projects, however some existing applications do run on the
2.x series).  While not required, this is the recommended method for 18F
employees to install Python.

# Installing Python Packages

Python packages are third-party modules written in and for Python and are
usually found within the [Python Package Index](https://pypi.python.org/pypi)
(PyPI).  There is a tool to use to help install and maintain them called
[`pip`](http://pip.readthedocs.org/en/stable/).  If you have at least Python
3.4 installed (or Python 2.7.9 if running the 2.x series), `pip` is
automatically installed with Python.  All other versions will require `pip` to
be [installed separately](http://pip.readthedocs.org/en/stable/installing/).

Another Python package installer exists called `easy_install`, but it is
being phased out in favor of `pip` especially now that `pip` is included with
Python itself.  If you see reference to `easy_install` in any Python package
installation instructions, try looking for a `pip` alternative to use instead.

In addition to installing packages from PyPI, `pip` can also install packages
directly from several types of source control repositories (git, Mercurial,
Subversion) or even from another location on your local machine.

## Using `pip`

`pip` is a simple way to install a Python package.  For instance, if you want
to install Django, you can do this:

`$ pip install django`

This will download and install the latest version of the Django package from
PyPI.  You can also install a specific version:

`$ pip install django==1.6`

To update a package to its latest version (including `pip` itself!), add a
flag to the install command:

`$ pip install -U django`

To uninstall a package, run this command:

`$ pip uninstall django`

Lastly, you can also install multiple packages at once from a file, which is
usually named `requirements.txt` or `requirements.pip` by community
convention:

`$ pip install -r requirements.txt`

This will install whatever packages are listed in the file.  For more
instructions and detailed usage information, please refer to the
[`pip` User Guide](http://pip.readthedocs.org/en/stable/user_guide/).

# Managing Project Dependencies

With Python installed and working with Python packages under your belt, there
is one last thing to discuss:  per-project dependencies.  When you install a
Python package, by default it will go into a directory called `site-packages`,
which lives a couple of levels down from wherever Python is installed on your
computer (commonly referred to as the "global `site-packages` directory").
This is undesirable, however, since different projects will likely require
different versions of these packages (which cannot both be installed in the
same place at the same time), and you might not have the appropriate
permissions for installing the packages in the first place, such as on a
shared host.

Instead, a better approach is to keep project package dependencies isolated
from each other and from the global `site-packages`.  This enables you to
install Python packages in user-controlled directories and prevent conflicts
when changing the versions of the packages from one project to another.
Python's solution for this problem is
[`virtualenv`](https://virtualenv.readthedocs.org/en/latest/).

## Python Virtual Environments with virtualenv

`virtualenv` enables Python to look for packages in locations outside of the
global `site-packages` directory, allowing you to import a specific directory
of packages into the project you're working on (`virtualenv` does this by
manipulating your `$PATH` environment variable).  When you create a new
virtual environment with `virtualenv`, it sets up a sub-directory with a
symlink to the Python interpreter along with its own `lib/site-packages`. It
also installs `pip` into the virtual environment.

This means that whenever you activate the virtual environment, any Python
packages that you install or modify are done so within the context of the
virtual environment itself, not the global `site-packages` directory.  This is
very powerful as it allows you to manage project dependencies independent of
one-another.

**Note:**  As of Python 3.4, `virtualenv` support is now built into the
language itself.  However, instead of `virtualenv` the command is now
[`pyvenv`](https://docs.python.org/3/library/venv.html).  If you are using
Python 3.4+, replace the `virtualenv` command with `pyvenv` instead in the
examples below.  If you are using an older version of Python, follow the
[`virtualenv` installation steps](https://virtualenv.readthedocs.org/en/latest/installation.html).

To create a new virtual environment for yourself, run this command:

`$ virtualenv my_new_env`

After it is created, you'll need to activate it to work within it:

```
$ cd my_new_env
$ source bin/activate
```

You are now working within the context of the virtual environment (regardless
of where you navigate to in your file system).

Installing a package within the virtual environment is a simple `pip` command
away:

`$ pip install django`

When you're done or ready to switch to another virtual environment, you should deactivate it first:

`$ deactivate`

## Managing Virtual Environments with virtualenvwrapper

In addition to `virtualenv`, another tool exists to help manage Python virtual
environments called
[`virtualenvwrapper`](https://virtualenvwrapper.readthedocs.org/en/latest/).
This tool makes it even easier to manage your virtual environments, including
giving you the ability to set up hooks for yourself to automatically
activate/deactivate them when navigating to/from a project directory that
contains one within it.  Alternatively, you can use a tool called
[`autoenv`](https://github.com/kennethreitz/autoenv) that does this for you
without the need to customize shell scripts yourself.

# Notable Exceptions for Global Python Packages

While most Python packages should be installed and managed within the context
of a virtual environment, there are a small handful of packages that might
need to be installed in the global `site-packages` directory.

First, if you are using a version of Python prior to 3.4 (or older than 2.7.9
if you are using the 2.x series), you will need to
[install `pip` manually](http://pip.readthedocs.org/en/stable/installing/).
In all versions of Python prior to version 3.4 you will also need to
[install `virtualenv`](https://virtualenv.readthedocs.org/en/latest/installation.html)
globally, as well as any `virtualenv` support tools you plan on using (e.g.,
`virtualenvwrapper` and `autoenv`).

IPython (now a sub-component of the Jupyter project) is another useful
exception as it is a much richer REPL (Read-Eval-Print Loop) to use when
working with Python, and if you're just testing something quick it is a bit of
a pain to have to get into a specific project.  You can read more about
IPython here:  http://ipython.org/

To install it, simply run the following command:

`$ pip install ipython`

This will install IPython globally and now you'll be able to just run
`ipython` without needing to activate a virtual environment to immediately get
into this powerful Python REPL.

# Useful Python Development Tools

This is a short list of Python packages that may come in handy when working on
a Python project.  The packages aid in the development process by providing
code coverage analysis, advanced debugging capabilities, more robust testing
support, and code style/formatting checks.

For more in-depth information regarding Python development, please check out
the [18F Development Guide](https://pages.18f.gov/development-guide/).

## Code Coverage

- [coverage.py](https://coverage.readthedocs.org/)

## Debugging

- [ipdb](https://github.com/gotcha/ipdb) (requires IPython)

## Linters

- [pep8](http://pep8.readthedocs.org/en/latest/)
- [pyflakes](https://github.com/pyflakes/pyflakes)
- [pylint](http://www.pylint.org/)
- [flake8](http://flake8.readthedocs.org/en/latest/)
  (a wrapper around pep8, pyflakes, and Ned Batchelder's McCabe script)

## Testing

- [pytest](http://pytest.org/latest/)
- [nose](https://nose.readthedocs.org/en/latest/)
- [mock](http://www.voidspace.org.uk/python/mock/) (for versions of Python
  prior to 3.3)
- [lettuce](http://lettuce.it/)

In addition, you may want to peruse the following bits of documentation around
testing with Python:

[18F's Python Testing Cookbook](https://pages.18f.gov/testing-cookbook/python/)
[Python Guide's Testing Your Code Article](http://docs.python-guide.org/en/latest/writing/tests/)

# Additional Reading

- [The Python Tutorial: Virtual Environments and Packages](https://docs.python.org/3/tutorial/venv.html) by the Python Software Foundation
- [PEP 0008 -- Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)
- [Automate your Python environment with pyenv](https://www.brianthicks.com/2015/04/10/pyenv-your-python-environment-automated/) by Brian Hicks
- [pyenv Tutorial](http://amaral-lab.org/resources/guides/pyenv-tutorial) by João Moreira
- [The Hitchhiker’s Guide to Python](http://docs.python-guide.org/) by Kenneth Reitz and others
