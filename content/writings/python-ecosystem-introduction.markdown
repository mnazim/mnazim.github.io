title: Python Ecosystem - An Introduction
author: Mir Nazim
published: 2011-11-28 03:00:00
tags: [ python, programming, tools, tutorial]
public: yes
chronological : yes
kind: writing
summary: |
    Are you a python developer
...

When developers shift from PHP, Ruby or any other platform to Python, the very first road block they face(most often) is the lack of overall understanding the Python ecosystem. Developers often yearn for a tutorial/resource that explains how to accomplish most tasks in a more or less standard way.

What follows, is an extract from the internal wiki at my [workplace][ikraft], that documents the basics of Python ecosystem for web application development for our interns, trainees and experienced developers who shift to Python from other platforms.

_This is not a complete resouces. My target is to make it **a work in perpetual progress**. Hopefully, over time, this will develop into an exhaustive tutorial._

## Intended Audience

This is not about teaching Python - the programming language. This tutorial will not magically transform you into a python ninja. I am assuming that you already know a basics of Python. If you don't, then stop right now. Go read Zed Shaw's brilliant free book [Learn Python The Hard Way][lpthw] first and then come back.

I am assuming you are working on Linux(preferably Ubuntu/Debian) or a Linux like operating system. Why? Because that is what I know best. I have not done any serious programming related work on MS Windows or Mac OS X, other than testing for cross browser compatibility. Checkout following tutorials on how to install Python on other platforms:

 - [Python 101: Setting up Python on Windows][pyonwin]
 - [Official documentation for Python on Windows][pyonwin_official]
 - [Official documentation for Python on Mac OS X][pyonmac_official]

Search the web for the best possible ways of installing Python on your operating system. I highly recommend asking on [Stack Overflow][stackoverflow]. 

## The version confusion

_**TL;DR:** Python 2.x is the status quo, Python 3 is the shiny new thing. If you don't care, just skip to **Installing Python** section below._

While starting with Python, installing version 3.x will seem like a natural first step, it might not be exactly what you want.

Currently there are two actively developed version of Python - 2.7.x and 3.x(also called Python 3, Py3K and Python 3000). Python 3 is a different language from Python 2. There a some subtle and some stark semantic and syntactic differences. As of today, Python 2.6/2.7 is most installed and most used version. Many mainstream and important packages/frameworks/tools/utilities/modules are not yet 100% compatible with Python 3.

Therefore, the safest choice would be to use 2.x(2.7.x to be more specific). Choose Python 3 only if you need to and/or fully understand the implications. 

[Python 3 Wall of Shame][py3wos] documents the Python 3 compatibility for various packages. Check it thoroughly before deciding to start on Python 3.

## Which VM to use

Python interpreter or the Python Virtual Machine has a number of different implementations, CPython being the main and most popular/installed implementation. CPython also acts as the reference implementation for other virtual machines. 

[PyPy][pypy] is Python implemented in Python, [Jython][jython] is implemented in Java and runs on Java VM and [IronPython][ironpython] is the Python implementation for Microsoft .Net CLR.

Unless it is really really important to choose otherwise, CPython should be used to avoid any surprises.

If all this gibber jabber about versions and virtual machines is giving you headaches, then all you need is CPython version 2.7.x. Trust me on this.

## Installing Python


Most of the Linux/Unix distros and Mac OS X come with Python pre-installed. If your's does not or has an older version, you can install version 2.7.x with following commands:

On Ubuntu/Debian and derivatives

    :::bash
    $ sudo apt-get install python2.7

_`sudo` is a program for Unix-like operating systems that allows users to run programs with the security privileges of another user (normally the superuser, or root). [You can learn more about sudo at Wikipeia][sudo]._

On Fedora/Red Hat and similar systems

    :::bash
    su
    yum install python2.7

From now onwards I will be using `sudo` in the examples; you should replace it with your distro specific equivalent.

## Understanding the packages 

First thing you need to understand that Python does not have any package management facilities by default. In fact, the very concept of packages in Python is very loose.

As you might already know, Python code is organized into modules. A module can be a single file containing just one function or a directory containing one or more sub-modules. The difference between a package and a module is very minimal and every module can be thought of as a package.

So what is the difference(if any) between a module and a package? For that you first need to understand how python finds the modules.

As with any programming environment, some functions and classes(`str`, `len`, `Exception`, etc.) in Python are available in the global scope and other need to be imported by means of an `import` statement. For example:

    :::python
    >>> import os
    >>> from os.path import basename, dirname

These packages must be somewhere in your filesystem so that they can be found by the `import` statement. How does Python know the location of these modules? These locations are set automatically when you install the Python virtual machine and are, almost always, dependent on the target platform.

The package path is always available for your inspection in `sys.path`. Here is what it looks like on my laptop which runs *Ubuntu 11.10 Oneric Ocelot*.

    :::python
    >>> import sys
    >>> print sys.path
    ['',
     '/usr/lib/python2.7',
     '/usr/lib/python2.7/plat-linux2',
     '/usr/lib/python2.7/lib-tk',
     '/usr/lib/python2.7/lib-old',
     '/usr/lib/python2.7/lib-dynload',
     '/usr/local/lib/python2.7/dist-packages',
     '/usr/lib/python2.7/dist-packages',
     '/usr/lib/python2.7/dist-packages/PIL',
     '/usr/lib/python2.7/dist-packages/gst-0.10',
     '/usr/lib/python2.7/dist-packages/gtk-2.0',
     '/usr/lib/pymodules/python2.7',
     '/usr/lib/python2.7/dist-packages/ubuntu-sso-client',
     '/usr/lib/python2.7/dist-packages/ubuntuone-client',
     '/usr/lib/python2.7/dist-packages/ubuntuone-control-panel',
     '/usr/lib/python2.7/dist-packages/ubuntuone-couch',
     '/usr/lib/python2.7/dist-packages/ubuntuone-installer',
     '/usr/lib/python2.7/dist-packages/ubuntuone-storage-protocol']

This will give you the list of directories where python will search for a given package. It starts at the top and keeps going downwards until a name match is found. This means if two different directories contain two packages with the same name, the package search will always stop at the first absolute match encountered and will never go further down the list.

You might have guessed by now, this package search path can  easily be hacked to make sure that python picks your packages first. All you need to do is:

    :::python
    >>> sys.path.insert(0, '/path/to/my/packages')

While this approach comes in handy in many situations, you must always bear in mind that it is very easy to abuse it. Use it if you have to but don't abuse it.

### The PYTHONPATH

`PYTHONPATH` is a environment variable that can be used to augment the default package search paths. Think of it as a `PATH` variable but specifically for Python. It is simply a list(not a Python list like `sys.path`) of directories containing the Python modules separated by `:`. It can be simply set as follows:

    :::bash
    export PYTHONPATH=/path/to/some/directory:/path/to/another/directory:/path/to/yet/another/directory

In some situations you will not want to overwrite the existing PYTHONPATH, just append or prepend to it.

    :::bash
    export PYTHONPATH=$PYTHONPATH:/path/to/some/directory    # Prepend
    export PYTHONPATH=/path/to/some/directory:$PYTHONPATH    # Append

Generally, you will put this inside the shell startup files like `.bashrc`, `.zshrc` etc.

In most cases, you should not need to set `PYTHONPATH` explicitly. There are more elegant way of accomplishing the same effect, which I will elaborate on in a moment.

Now that you understand how python find the installed packages, we can revisit our original question. What is the difference between a module and a package? Package is just a module or a collection of modules/sub-modules, generally comes compressed inside a tarball, which contains 1) information on dependencies - if any 2) instructions to copy the files to the standard package search location and 3) compile instructions - if it contains code that must be compiled before installation. That's it.

## Third party packages

Right from the start, for any serious programming with Python you will need to install third party packages for various tasks.

On a Linux System there are at least 3 ways of installing the third party packages.

 1. using the package management system of your distro(deb, rpm, etc.)
 2. by means of various community developed tools `pip`, `easy_install`, etc.
 3. installing from the source files.

All the three ways, almost always, accomplish the same thing _viz._ install dependencies, compile code if needed and copy the modules contained inside a package to the standard package search locations.

While #2 and #3 will work almost unchanged on any operating system, I will once again point you to [Stack Overflow][stackoverflow] to find out other ways of installing third party packages on your target platform.

### Where to find third party packages

Before you can install third party packages, you will have to find them. There are more than a few ways of finding packages.

 1. the modules packaged for your distro specific package management system.
 2. [Python Package Index(or PyPI)][pypi]
 3. Various source code hosting services like [Launchpad][launchpad], [GitHub][github], [BitBucket][bitbucket], etc.

### Installing with distro specific package management system

Installing packages using the distro package management system is simply issuing the command or using whatever GUI app you use to install other apps. E.g to install `simplejson` a JSON parsing utility on an Ubuntu system, you will issue following command:

    :::bash
    $ sudo apt-get install python-simplejson

### Installing with pip

*`easy_install` has mostly fallen out of favor. We will only concentrate on `pip`, which is a replament for `easy_install`*

`pip` is a tool for installing and managing Python packages, such as those found in the Python Package Index. `pip` is not installed with the Python virtual machine therefore we need to install it first. On Linux, I generally install it as follows:

    :::bash
    $ sudo apt-get install python-pip

Before installing any other package I always upgrade `pip` to the latest version available in PyPI as Ubuntu repositories are generally behind PyPI. I upgrade `pip` with pip itself.

    :::bash
    $ sudo pip install pip --upgrade

Now to install any python package you run `pip install package-name` command. Therefore, to install `simplejson` you will run following command.

    :::bash
    $ sudo pip install simplejson

Removing packages is equally easy with it.

    :::bash
    $ sudo pip uninstall simplejson

By default, `pip` will install the most recent stable version as found on PyPI, but often you will face situations where you will want to install a specific version of a package, because your project depends on that specific version. To accomplish that you will use the `pip install` command as follows:

    :::bash
    $ sudo pip install simplejson==2.2.1

You will often want to upgrade, downgrade and/or reinstall packages. It can be done with following commands.

    :::bash
    $ sudo pip install simplejson --updrage         # Upgrade a package to the latest version from PyPI
    $ sudo pip install simplejson==2.2.1 --updrage  # Upgrade/downgrade a package to a given version

Now what if you want to install the development version of a package which is in a version control repository and not yet on PyPI. `pip` takes care of that as well, but before you can do that, you will need to install the version control systems itself. On Ubuntu, you will install as follows.

    :::bash
    $ sudo apt-get install git-core mercurial subversion

Once you have version control systems installed, installing a package from a version control repository can be done as follows:

    :::bash
    $ sudo pip install git+http://somedomain.com/path/to/git-repo#egg=packagename
    $ sudo pip install hg+http://somedomain.com/path/to/hg-repo#egg=packagename
    $ sudo pip install svn+http://somedomain.com/path/to/svn-repo#egg=packagename

Now you might be wondering what going on with these is *eggs*. Right now all you need to understand is that an egg is a zipped python package containing package source and some metadata. `pip` builds the egg information before it installs the package. You can find the egg name by inspecting the `setup.py` file within the code repository(it will almost always be there). Find the `setup` section and look for the line that looks like `name="something"`. It will look somewhat similar to following piece of code(taken from the `setup.py` file from simplejson).

    :::python
    setup(
        name="simplejson", # <--- This is your egg name
        version=VERSION,
        description=DESCRIPTION,
        long_description=LONG_DESCRIPTION,
        classifiers=CLASSIFIERS,
        author="Bob Ippolito",
        author_email="bob@redivi.com",
        url="http://github.com/simplejson/simplejson",
        license="MIT License",
        packages=['simplejson', 'simplejson.tests'],
        platforms=['any'],
        **kw)

What if there is no `setup.py` file? How do you find the egg name? Well you don't need to. Just copy the package source to you project directory and import and use just like you would use your own code.

### Installing from source.

Installing a python package from source is just one command. Extract the package content into a directory and run following command:

    :::bash
    cd /path/to/package/directory
    python setup.py install

Although you can use this method of installation with no real difference at all, but understand that `pip` is always the recommended way of installing the packages because `pip` gives you ability to upgrade/downgrade the packages very easily without the extra work involved in manually downloading, extracting and installing. Installing from source should always be you last option; if everything else fails(which normally should never fail).

### Install packages that need compiling 

While we have covered most of what there is about installing packages, there is one thing we have not covered. The Python packages containing C/C++ code which will need to be compiled before they can be installed and used. The best example of such packages are database adapters, image processing libraries etc.

While pip can manage the compilation of sources, I personally prefer to install such packages using the distro specific package management system, which installs the prebuilt binaries.

If you still want/need to install with `pip`, here is what you will need on an Ubuntu system.

Compiler and related tools.

    :::bash
    $ sudo apt-get install build-essential

Python development files(headers etc.).

    :::bash
    $ sudo aptitude install python-dev-all

Assuming you are installing `psycopg2` - the PostgreSQL RDBMS adaptor for Python, you will also need the development files for PostgreSQL.

    :::bash
    $ sudo aptitude install  postgresql-server-dev-all

Once these dependencies are met, you can now run `pip install`.

    :::bash
    $ sudo pip install psycopg2

There is one thing that should be remembered: ***Not all such packages are compatible with `pip` installation method***. But if you feel confident about compiling the sources and/or already have the necessary experience/understanding of how all this works on your target platform - by all means go ahead and install however the way you want.

## The Development Environment

Different people like to setup their development environment in different ways, but in almost all programming communities, one(or more than one) way of setting up the development environment is more accepted than others. While there is nothing wrong in setting up your development environment differently, but generally these methods/setups are more tested and known to make some repetitive/biolerplate tasks in day to day work easy and maintainable.

### virtualenv
The most popular method of setting up the development environment, in Python, is using the **virtualenv** package. Virtualenv is a tool to create isolated python environments. Now the question arises why do we need isolated python environment? To answer that I quote the virtualenv documentation itself.

   > The basic problem being addressed is one of dependencies and versions, and indirectly permissions. Imagine you have an application that needs version 1 of LibFoo, but another application requires version 2. How can you use both these applications? If you install everything into /usr/lib/python2.7/site-packages (or whatever your platform's standard location is), it's easy to end up in a situation where you unintentionally upgrade an application that shouldn't be upgraded.

In simple words, you can have different/isolated python environments for each of your projects; you will install required packages for each of your projects into its own isolated environment.

Use `pip` to install virtualenv as well.

    :::bash
    $ sudo pip install virtualenv

Now once virtualenv is installed, run following commands to create an isolated python environment for your project.

    :::bash
    $ mkdir my_project_venv
    $ virtualenv --distribute my_project_venv
    # The output will something like:
    New python executable in my_project_venv/bin/python
    Installing distribute.............................................done.
    Installing pip.....................done.

So what's happening here? You created a directory called `my_project_venv` to hold your new isolated python environment. The `--distribute` tell virtualenv to use new/improved packaging system based on `distribute` package instead of using old system based on `setuptools`. All you need to understand right now is that `--distribute` option will install `pip` automatically within the new virtual environment so you do not have to. As your knowledge/experience as a python developer increase, you will start better understanding these under the hood nuts and bolts.

Now inspect the contents of `my_project_venv` directory, you will see a structure similar to:

    :::bash
    # Showing only files/directories relevant to the discussion at hand
    .
    |-- bin
    |   |-- activate  # <-- Activates this virtualenv
    |   |-- pip       # <-- pip specific to this virtualenv
    |   `-- python    # <-- A copy of python interpreter
    `-- lib
        `-- python2.7 # <-- This is where all new packages will go

Activate the virtualenv with following command:

    :::bash
    $ cd my_project_venv
    $ source bin/activate

After *sourcing* the `activate` script, your prompt should look something like this:
    :::bash
    (my_project_venv)$ # the virtualenv name prepended to the prompt

Now deactivate the virtualenv with following command:
    :::bash
    (my_project_venv)$ deactivate

Run following commands to better understand the difference between the system wide installation(deactivate the virtualenv first, if it is active).

First let's find out which python/pip executable will be used if I called `python` or `pip` from terminal.

    :::bash
    $ which python
    /usr/bin/python
    $ which pip
    /usr/local/bin/pip
_Learn about `which` command at [Wikipeia][whichcommand]._

Now do it again, but activate the virtualenv first and note the differences in the output. On my machine it looks like:

    :::bash
    $ cd my_project_venv
    $ source bin/activate
    (my_project_venv)$ which python
    /home/mir/my_project_venv/bin/python
    (my_project_venv)$ which pip
    /home/mir/my_project_venv/bin/pip

What `virtualenv` did, is make a copy python of executable, create a few utility scripts and a place to install project specific packages that you will eventually install/upgrade/remove over the life time of the project. It also did some package search path/PYTHONPATH magic to ensure that 1) when you install packages, they are installed inside the currently active virtualenv and not the system wide python installation and 2) when imported from code, the packages in the currently active virtualenv will take precedence over the ones installed in system wide Python installations.

An important thing to note here is that, by default all the packages installed inside the system wide python are automatically available to the virtualenv. That means if you installed the `simplejson` package in your system wide python installation it automatically will be available to the all the virtualenvs. This behaviour can be altered by adding a `--no-site-packages` switch at the time of creation of virtualenv, like:

    :::bash
    $ virtualenv my_project_venv --no-site-packages

### virtualenvwrapper

`virtualenvwrapper` is a wrapper around `virtualenv` which provides some really nice utilities to create/activate/manage/destroy virtual environments, which otherwise will be chore. To install `virtualenvwrapper`, run following command:

    :::bash
    $ sudo pip install virtualenvwrapper

Once installed, you will need to configure it. Here is how I configure it.

    :::bash
    if [ `id -u` != '0' ]; then
      export VIRTUALENV_USE_DISTRIBUTE = 1        # <-- Always use pip/distribute
      export WORKON_HOME=$HOME/.virtualenvs       # <-- Where all virtualenvs will be stored
      source /usr/local/bin/virtualenvwrapper.sh
      export PIP_VIRTUALENV_BASE = $WORKON_HOME
      export PIP_RESPECT_VIRTUALENV = true
    fi

Logout once and login again for the configuration to take effect. 

Now to create/activate/deactivate/delete a virutalenv, you will run following [self explainatory]command.

    :::bash
    $ mkvirtualenv my_project_venv
    $ workon my_project_venv
    $ deactivate
    $ rmvirtualenv my_project_venv

*Tab based, bash shell command completion also works with virtualenvwrapper.*

Go over to [virtualenvwrapper homepage][virtualenvwrapper] to learn more about available commands and configuration options.

### Basic dependency management with pip and virtualenv

`pip` in combination with `virtualenv` can provide basic dependency management facilities for your project. 

You can use `pip freeze` command to export the list of currently installed packages. For example, here is the list of python packages I use to build this blog: 

    :::bash
    $ pip freeze -l 
    Jinja2==2.6
    PyYAML==3.10
    Pygments==1.4
    distribute==0.6.19
    markdown2==1.0.1.19

_Note the `-l` switch. It tells `pip` to export only the packages installed in currently active virtual environment and exclude the globally installed packages from the list_

You can save this exported list to a file and add it to you version control system.

    :::bash
    $ pip freeze -l  > requirements.txt

`pip` can also install packages from a file containing the output of the `pip freeze` command.

    :::bash
    $ pip install -r requirements.txt


## Other important tools

While we covered the basics of python versions, VMs and package management, there are tasks in day to day work, other than these which require special purpose tools to accomplish. While I cannot go into every bit of detail for each of the tool but I will try give the overview. 

_Apologies in advance, as most of the tools are specific to web application developers._

### The Editor

There are quite a number of good editors which provide tools for programming in Python. I, personally, am biased towards Vim, but I do not intend to start the _Editor Wars_ - or do I ;).

Some good editors and IDEs(if you like IDEs) that have support for python programming are Vim/Gvim, Emacs, GEdit for GNOME, Kate for KDE, Scribes, Komodo Edit/IDE from ActiveState, Wing IDE from WingWare, PyCharm from JetBrains, PyDEV for Eclipse. There are others but these seem to be the most popular ones. Choose whichever works best for you.


### Pyflakes: Source checking and linting

Pyflakes is a simple program which checks Python source files for errors by analyzing the text of the file. It checks for syntax and [some]logical errors, modules imported but unused, variables used only once etc.

You can install it with `pip`:

    :::bash
    $ pip install pyflakes

Call it from the terminal with a python source code file as argument, like:

    :::bash
    $ pyflakes filename.py

Pyflakes can be integrated in your editor as well. Here is how it looks on my Vim. Note the _red squigly lines_.

[![PyFlakes][pyflakes.png]][pyflakes.png]

Ask on [Stack Overflow][stackoverflow] how to add Pyflakes support for the editor of your choice.

[Pyflakes website][pyflakes]

### Requests: HTTP library for human beings 

Requests is a library that makes working with HTTP a breeze. 

Install it with `pip`, like:

    :::bash
    $ pip install requests

Here is a simple example.

    :::python
    >>> import requests
    >>> r = requests.get('https://api.github.com', auth=('user', 'pass'))
    >>> r.status_code
    204
    >>> r.headers['content-type']
    'application/json'
    >>> r.content
    ... 
[Requests documentation][requests_docs]

### Flask: Microframework for web development

Flask is a microframework for Python based on Werkzeug, Jinja 2. 

Install it with `pip`
   
    :::bash
    $ pip install Flask

Here is a simple example.

    :::python
    from flask import Flask
    app = Flask(__name__)
    
    @app.route("/")
    def hello():
        return "Hello World!"
    
    if __name__ == "__main__":
        app.run()

Run it like:

    :::bash
    $ python hello.py
     * Running on http://localhost:5000/

[Flask website][flask]

### Django: Full stack framework for web development

Django is a full stack web framework. It provides an ORM, HTTP library, form handling, XSS filtering, templating among other things. 

Install will `pip`, like:

    :::bash
    $ pip install Django

Head over [Django website][django] and follow the documentation to learn it. It's quite easy.

### Fabric: Streamline the use of SSH for deployment and/or system admin tasks

Fabric is a Python library and command-line tool for streamlining the use of SSH for application deployment or systems administration tasks.

It provides a basic suite of operations for executing local or remote shell commands (normally or via sudo) and uploading/downloading files, as well as auxiliary functionality such as prompting the running user for input, or aborting execution.

You can install fabfile with `pip`

    :::bash
    $ pip install fabric

Here is a simple task written with fabric:

    :::python
    from fabric.api import run
    
    def host_type():
        run('uname -s')

Then you execute the ask on one or more servers like:

    :::bash
    $ fab -H localhost host_type
    [localhost] run: uname -s
    [localhost] out: Linux
    
    Done.
    Disconnecting from localhost... done.

[Fabric website][fabric]

### SciPy: tools for scientific computing with Python

If your work involves scientific and numerical computing, the SciPy is an indispensable tool for you.

From SciPy website:

> SciPy (pronounced "Sigh Pie") is open-source software for mathematics, science, and engineering. It is also the name of a very popular conference on scientific programming with Python. The SciPy library depends on NumPy, which provides convenient and fast N-dimensional array manipulation. The SciPy library is built to work with NumPy arrays, and provides many user-friendly and efficient numerical routines such as routines for numerical integration and optimization. Together, they run on all popular operating systems, are quick to install, and are free of charge. NumPy and SciPy are easy to use, but powerful enough to be depended upon by some of the world's leading scientists and engineers. If you need to manipulate numbers on a computer and display or publish the results, give SciPy a try! 

Head over to [SciPy website][scipy] for detailed download/install instructions and documentation.


### PEP 8: Python Style Guide

While not a software tool _per se_, PEP 8 is a very important resource related to Python.

PEP 8 is a document that defines coding conventions for the Python code comprising the standard library in the main Python distribution. The sole purpose of the document is to ensure that the python code from everywhere follows same physical layout of the code, naming patterns for vaiables, class and function names. Make sure you understand it thoroughly and follow it. It will ease you life a lot over time.

[PEP 0008][pep8]

### The Mighty Python Standard Library

Python's standard library is very extensive, offering a wide range of facilities. The library contains built-in modules (written in C) that provide access to system functionality such as file I/O, as well as modules written in Python that provide standardized solutions for many problems that occur in everyday programming. Some of these modules are explicitly designed to encourage and enhance the portability of Python programs by abstracting away platform-specifics into platform-neutral APIs.

Checkout the [Official documentation for the Standard Library][pylib].


## Parting thought

What I have covered so far in this tutorial, is just skimming the surface. There is a vast sea of useful tools, libraries and software available for Python for almost every concievable task and which cannot covered in a single go. You will have to discover them yourself, over time.

Python has a great community of really very smart people, who have a very patient and helpful attitude towards new commers to the language. So hang out at IRC channels of your favorite open source projects, follow and ask in mailing lists, talk to people who have experience in implementing small and large systems in Python(and others). Overtime, as your experience/knowledge expands you will become one of them yourself.

I leave you with the **Zen Of Python**. Ponder. Contemplate. Be Enlightened! _Happy Pythoning_

    :::text
     1. Beautiful is better than ugly.
     2. Explicit is better than implicit.
     3. Simple is better than complex.
     4. Complex is better than complicated.
     5. Flat is better than nested.
     6. Sparse is better than dense.
     7. Readability counts.
     8. Special cases aren't special enough to break the rules.
     9. Although practicality beats purity.
    11. Errors should never pass silently.
    12. Unless explicitly silenced.
    13. In the face of ambiguity, refuse the temptation to guess.
    14. There should be one-- and preferably only one --obvious way to do it.
    15. Although that way may not be obvious at first unless you're Dutch.
    16. Now is better than never.
    17. Although never is often better than *right* now.
    18. If the implementation is hard to explain, it's a bad idea.
    19. If the implementation is easy to explain, it may be a good idea.
    20. Namespaces are one honking great idea -- let's do more of those!





 [lpthw]: http://learnpythonthehardway.org/
 [pyonwin]:http://www.blog.pythonlibrary.org/2011/11/24/python-101-setting-up-python-on-windows/
 [pyonwin_official]:http://docs.python.org/using/windows.html
 [pyonmac_official]:http://docs.python.org/using/mac.html
 [py3wos]:http://python3wos.appspot.com/
 [pywin]: http://
 [stackoverflow]: http://www.stackoverflow.com
 [pypi]:http://pypi.python.org/pypi
 [github]:http://github.com
 [launchpad]:https://launchpad.net/
 [bitbucket]:https://bitbucket.org/
 [ikraft]:http://ikraftsoft.com/
 [sudo]:http://en.wikipedia.org/wiki/Sudo
 [pypy]:http://pypy.org/ 
 [jython]:http://www.jython.org/ 
 [ironpython]:http://ironpython.net/ 
 [whichcommand]:http://en.wikipedia.org/wiki/Which_%28Unix%29 
 [fabric]:http://fabfile.org 
 [pyflakes]:https://launchpad.net/pyflakes 
 [pyflakes.png]:/media/img/content/vim-pyflakes.png "Pyflakes"
 [requests_docs]:http://docs.python-requests.org/en/latest/index.html 
 [flask]:http://flask.pocoo.org/ 
 [django]: http://djangoproject.com
 [scipy]:http://www.scipy.org/ 
 [pylib]:http://docs.python.org/library/   
 [pep8]:http://www.python.org/dev/peps/pep-0008/ 
 [virtualenvwrapper]:http://www.doughellmann.com/projects/virtualenvwrapper/ 
