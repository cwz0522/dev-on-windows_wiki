## Interpreter

Python is a great programming language, and used in many tools.

Luckily, installing it is very easy. Go to the [Python downloads](https://www.python.org/downloads/) and download the latest version of both Python 2.x and Python 3.x, and run both installers, Python 3.x first. Choose install for all users unless you know for sure you want the other option.

Then, by default it will suggest to install in `C:\PythonXY\`. Install in `C:\dev\PythonXY\` instead.

The installer will ask you what features you want installed. Select all, and also make sure you tick _'Add python.exe to Path'_.

Manually make sure Python3 (and it's Scripts directory) comes __after__ Python2 in the `PATH`. Then go to your Python3 installation directory and __copy__ `python.exe` and `pythonw.exe` to respectively `python3.exe` and `python3w.exe`.

Now you can use `python` for Python 2, `python3` for Python3.

## Packages

The latest version of both Python 2.x and 3.x should come with `pip`, the package manager. `pip` is for Python2, `pip3` is for Python3. To build a lot of packages you will need to have Visual Studio installed. As of writing, Python2 uses Visual C++ 2008, and Python3 uses 2010.

I can also suggest [`pipwin`](https://pypi.python.org/pypi/pipwin/). It's a small pure-Python package that makes it really easy to install the excellent binary packages maintained by [Christoph Gohlke](http://www.lfd.uci.edu/~gohlke/pythonlibs/). Make sure for Python3 to go to your Python's scripts folder (`C:\Python3X\Scripts`) and __copy__ `pipwin.exe` to `pipwin3.exe`.