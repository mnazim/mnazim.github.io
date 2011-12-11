title: Python Ecosystem - An Introduction
author: Mir Nazim
published: 2011-11-28 03:00:00
tags: [ python, programming, tools, tutorial ]
public: yes
chronological : yes
kind: writing
discussions_link: http://news.ycombinator.com/item?id=3286399 
summary: |
    When developers shift from PHP, Ruby or any other platform to Python, the very first road block they face (most often) is a lack of an overall understanding of the Python ecosystem. Developers often yearn for a tutorial or resource that explains how to accomplish most tasks in a more or less standard way.
credits: |
    My gratitude to the the people who provided valuable feedback over the Hacker News discussion and via direct email. A special thanks to [Nathaniel Guy](http://www.natguy.net) who took out time and actually sent me a patch with improvements in grammar, naration and other some other fixes.
...

When developers shift from PHP, Ruby or any other platform to Python, the very first road block they face (most often) is a lack of an overall understanding of the Python ecosystem. Developers often yearn for a tutorial or resource that explains how to accomplish most tasks in a more or less standard way.

What follows is an extract from the internal wiki at my [workplace][ikraft], which documents the basics of the Python ecosystem for web application development for our interns, trainees and experienced developers who shift to Python from other platforms.

_This is not a complete resource. My target is to make it **a work in perpetual progress**. Hopefully, over time, this will develop into an exhaustive tutorial._

## Intended Audience

This is not about teaching Python - the programming language. This tutorial will not magically transform you into a Python ninja. I am assuming that you already know the basics of Python. If you don't, then stop right now. Go read Zed Shaw's brilliant free book [Learn Python The Hard Way][lpthw] first and then come back.

I am assuming you are working on Linux (preferably Ubuntu/Debian) or a Linux-like operating system. Why? Because that is what I know best. I have not done any serious programming related work on MS Windows or Mac OS X, other than testing for cross-browser compatibility. Check out the following tutorials on how to install Python on other platforms:

 - [Python 101: Setting up Python on Windows][pyonwin]
 - [Official documentation for Python on Windows][pyonwin_official]
 - [Official documentation for Python on Mac OS X][pyonmac_official]

Search the web for the best possible ways of installing Python on your operating system. I highly recommend asking on [Stack Overflow][stackoverflow]. 

## The version confusion

_**TL;DR:** Python 2.x is the status quo; Python 3 is the shiny new thing. If you don't care, just skip to **Installing Python** section below._

When starting with Python, installing version 3.x will seem like a natural first step, but it might not be exactly what you want.

Currently there are two actively developed versions of Python - 2.7.x and 3.x (also called Python 3, Py3K and Python 3000). Python 3 is a different language from Python 2. There are some subtle and some stark semantic and syntactic differences. As of today, Python 2.6/2.7 is the most installed and most used version. Many mainstream and important packages/frameworks/tools/utilities/modules are not yet 100% compatible with Python 3.

Therefore, the safest choice would be to use 2.x (2.7.x to be more specific). Choose Python 3 only if you need it and/or fully understand the implications.

[Python 3 Wall of Shame][py3wos] documents the Python 3 compatibility for various packages. Check it thoroughly before deciding to start with Python 3.

## Which VM to use

The Python interpreter or the Python Virtual Machine has a number of different implementations, CPython being the main and most popularly installed implementation. CPython also acts as the reference implementation for other virtual machines. 

[PyPy][pypy] is Python implemented in Python, [Jython][jython] is implemented in Java and runs on the Java VM and [IronPython][ironpython] is the Python implementation for Microsoft .NET CLR.

Unless it is really, really important to choose otherwise, CPython should be used to avoid any surprises.

If all this jibber jabber about versions and virtual machines is giving you headaches, then all you need is CPython version 2.7.x. Trust me on this.

## Installing Python

Most of the Linux/Unix distros and Mac OS X come with Python pre-installed. If yours does not or has an older version, you can install version 2.7.x with the following command:

On Ubuntu/Debian and derivatives

    :::bash
    $ sudo apt-get install python2.7

_`sudo` is a program for Unix-like operating systems that allows users to run programs with the security privileges of another user (normally the superuser, or root). [You can learn more about sudo at Wikipedia][sudo]._

On Fedora/Red Hat and similar systems

    :::bash
    sudo yum install python2.7

*On RHEL you would likely need EPEL repositories enabled for install to work*

From this point on, I will be using `sudo` in my examples; you should replace this with your distro-specific equivalent.

## Understanding the packages 

The first thing you need to understand is that Python does not have any package management facilities by default. In fact, the very concept of packages in Python is very loose.

As you might already know, Python code is organized into modules. A module can be a single file containing just one function or a directory containing one or more sub-modules. The difference between a package and a module is very minimal and every module can be thought of as a package.

So what is the difference (if any) between a module and a package? For that you first need to understand how Python finds the modules.

As with any programming environment, some functions and classes (`str`, `len`, `Exception`, etc.) in Python are available in the global scope(called `builtin` scope in Python) and others need to be imported by means of an `import` statement. For example:

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

This will give you the list of directories where Python will search for a given package. It starts at the top and keeps going downwards until a name match is found. This means if two different directories contain two packages with the same name, the package search will always stop at the first absolute match encountered and will never go further down the list.

As you might have guessed by now, this package search path can easily be hacked to ensure that Python picks your packages first. All you need to do is:

    :::python
    >>> sys.path.insert(0, '/path/to/my/packages')

While this approach comes in handy in many situations, you must always bear in mind that it is very easy to abuse it. **Use it if you have to but don't abuse it**.

The `site` module controls the method by which these package search paths are set. It is imported automatically at the time of initialization of Python virtual machine. If you would like to understand the process involved in more detail, head over to it's [official documentation page][site_module].

### The PYTHONPATH

`PYTHONPATH` is a environment variable that can be used to augment the default package search paths. Think of it as a `PATH` variable but specifically for Python. It is simply a list (not a Python list like `sys.path`) of directories containing the Python modules separated by `:`. It can be simply set as follows:

    :::bash
    export PYTHONPATH=/path/to/some/directory:/path/to/another/directory:/path/to/yet/another/directory

In some situations you will not want to overwrite the existing `PYTHONPATH`, but just append or prepend to it.

    :::bash
    export PYTHONPATH=$PYTHONPATH:/path/to/some/directory    # Append
    export PYTHONPATH=/path/to/some/directory:$PYTHONPATH    # Prepend

Generally, you will put this inside the shell startup files like `.bashrc`, `.zshrc`, etc.

**`PYTHONPATH`, `sys.path.insert` and similar techniques are hack and generally it is better to stay away for these hacks. Use them, if they solve your local development environment problems but your production environments should not depend on such hacks. There are more elegant ways of accomplishing the same effect, on which I will elaborate in a moment**.

Now that you understand how Python finds the installed packages, we can revisit our original question. What is the difference between a module and a package? A package is just a module or a collection of modules/sub-modules, and generally comes compressed inside a tarball, which contains 1) information on dependencies - if any 2) instructions to copy the files to the standard package search location and 3) compile instructions - if it contains code that must be compiled before installation. That's it.

## Third party packages

Right from the start, for any serious programming with Python you will need to install third party packages for various tasks.

On a Linux System there are at least 3 ways of installing third party packages.

 1. using the package management system of your distro (deb, rpm, etc.)
 2. by means of various community-developed tools like `pip`, `easy_install`, etc.
 3. installing from the source files

All three ways, almost always, accomplish the same thing _viz._ install dependencies, compile code if needed and copy the modules contained inside a package to the standard package search locations.

While #2 and #3 will work almost unchanged on any operating system, I will once again point you to [Stack Overflow][stackoverflow] to find out other ways of installing third party packages on your target platform.

### Where to find third party packages

Before you can install third party packages, you will have to find them. There are more than a few ways of finding packages.

 1. the modules packaged for your distro-specific package management system
 2. [Python Package Index (or PyPI)][pypi]
 3. Various source code hosting services like [Launchpad][launchpad], [GitHub][github], [BitBucket][bitbucket], etc.

### Installing with distro-specific package management systems

Installing packages using the distro package management system is simply issuing the command or using whatever GUI app you use to install other apps. E.g., to install `simplejson` (a JSON parsing utility) on an Ubuntu system, you would issue the following command:

    :::bash
    $ sudo apt-get install python-simplejson

### Installing with pip

*`easy_install` has mostly fallen out of favor. We will only concentrate on `pip`, which is a replacement for `easy_install`.*

`pip` is a tool for installing and managing Python packages, such as those found in the Python Package Index. `pip` is not installed with the Python virtual machine, therefore we need to install it first. On Linux, I generally install it as follows:

    :::bash
    $ sudo apt-get install python-pip

Before installing any other package I always upgrade `pip` to the latest version available in PyPI as Ubuntu repositories are generally behind PyPI. I upgrade `pip` with pip itself.

    :::bash
    $ sudo pip install pip --upgrade

Now, to install any python package, you would run the `pip install package-name` command. Therefore, to install `simplejson` you would run the following command:

    :::bash
    $ sudo pip install simplejson

Removing packages is just as easy.

    :::bash
    $ sudo pip uninstall simplejson

By default, `pip` will install the most recent stable version found on PyPI, but often you will face situations where you will want to install a specific version of a package, because your project depends on that specific version. To accomplish that you will use the `pip install` command as follows:

    :::bash
    $ sudo pip install simplejson==2.2.1

You will often want to upgrade, downgrade and/or reinstall packages. This can be done with the following commands.

    :::bash
    $ sudo pip install simplejson --upgrade         # Upgrade a package to the latest version from PyPI
    $ sudo pip install simplejson==2.2.1 --upgrade  # Upgrade/downgrade a package to a given version

Now, what if you want to install the development version of a package which is in a version control repository and not yet on PyPI? `pip` takes care of that as well, but before you can do this, you will need to install the version control systems itself. On Ubuntu, you would perform the installation as follows:

    :::bash
    $ sudo apt-get install git-core mercurial subversion

Once you have the version control system installed, installing a package from a version control repository can be done as follows:

    :::bash
    $ sudo pip install git+http://hostname_or_ip/path/to/git-repo#egg=packagename
    $ sudo pip install hg+http://hostname_or_ip/path/to/hg-repo#egg=packagename
    $ sudo pip install svn+http://hostname_or_ip/path/to/svn-repo#egg=packagename

You could install from a repository, equally easily. Note the triple slash in the filesystem path.

    :::bash
    $ sudo pip install git+file:///path/to/local/repository

One thing should be noted that while installing via `git` protocol, you should `git+git` prefix like so:

    :::bash
    $ sudo pip install git+git://hostname_or_ip/path/to/git-repo#egg=packagename

Now, you might be wondering what going on with these *eggs*. Right now all you need to understand is that an egg is a zipped Python package containing package source and some metadata. `pip` builds the egg information before it installs the package. You can find the egg name by inspecting the `setup.py` file within the code repository (it will almost always be there). Find the `setup` section and look for the line that looks like `name="something"`. It will look somewhat similar to the following piece of code (taken from the `setup.py` file from simplejson).

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

What if there is no `setup.py` file? How do you find the egg name? Well, you don't need to. Just copy the package source to your project directory and import and use it just like you would use your own code.

#### The --user switch

All of the above examples will install the packages system-wide. If you use `--user` switch with `pip install`, the packages will be installed in your `~/.local` directory. For example, on my machine, it looks like this:

    :::bash
    $ pip install --user markdown2
    Downloading/unpacking markdown2
      Downloading markdown2-1.0.1.19.zip (130Kb): 130Kb downloaded
      Running setup.py egg_info for package markdown2
        
    Installing collected packages: markdown2
      Running setup.py install for markdown2
        warning: build_py: byte-compiling is disabled, skipping.
        
        changing mode of build/scripts-2.7/markdown2 from 664 to 775
        warning: install_lib: byte-compiling is disabled, skipping.
        
        
        changing mode of /home/mir/.local/bin/markdown2 to 775
    Successfully installed markdown2
    Cleaning up...

*Note the filesystem location (`/home/mir/.local/bin/markdown2`) where the markdown2 package was installed.*

There are a number of reasons for why you would not want to install all the packages in the system-wide locations. I will go over them in a moment when I show you how to set up separate and isolated Python environments for each of your projects.

### Installing from source.

Installing a python package from source is just one command. Extract the package content into a directory and run the following commands:

    :::bash
    cd /path/to/package/directory
    python setup.py install

Although you can use this method of installation with no real difference at all, understand that `pip` is always the recommended way of installing the packages because `pip` gives you the ability to upgrade/downgrade the packages very easily without the extra work involved in manually downloading, extracting and installing. Installing from source should always be your last option, if all else fails (which it normally should not).

### Install packages that need compiling 

While we have covered most of what there is to installing packages, there is one thing we have not covered: Python packages containing C/C++ code which need to be compiled before they can be installed and used. The best examples of such packages are database adapters, image processing libraries, etc.

While pip can manage the compilation of source, I personally prefer to install such packages using the distro-specific package management system, which installs the pre-built binaries.

If you still want/need to install with `pip`, here is what you will need on an Ubuntu system.

Compiler and related tools:

    :::bash
    $ sudo apt-get install build-essential

Python development files (headers, etc.):

    :::bash
    $ sudo aptitude install python-dev-all 

If your distribution does not have `python-dev-all`, look for packages with names similar to `python-dev`, `python2.X-dev`, etc. 

Assuming you are installing `psycopg2` (the PostgreSQL RDBMS adapter for Python), you will also need the development files for PostgreSQL.

    :::bash
    $ sudo aptitude install  postgresql-server-dev-all

Once these dependencies are satisfied, you can now run `pip install`.

    :::bash
    $ sudo pip install psycopg2

There is one thing that should be remembered: ***Not all such packages are compatible with the `pip` installation method***. But if you feel confident about compiling the source and/or already have the necessary experience/understanding of how all this works on your target platform, then by all means go ahead and install however you want.

## The Development Environment

Different people like to set up their development environment in different ways, but in almost all programming communities, one (or more than one) way of setting up the development environment is more accepted than others. While there is nothing wrong with setting up your development environment differently, generally these methods/setups are more tested and known to make some repetitive/boilerplate tasks in day-to-day work easy and maintainable.

### virtualenv
The most popular method of setting up the development environment in Python is using the **virtualenv** package. Virtualenv is a tool to create isolated Python environments. Now, the question arises: why do we need an isolated Python environment? To answer that, allow me to quote the virtualenv documentation itself.

   > The basic problem being addressed is one of dependencies and versions, and indirectly permissions. Imagine you have an application that needs version 1 of LibFoo, but another application requires version 2. How can you use both these applications? If you install everything into /usr/lib/python2.7/site-packages (or whatever your platform's standard location is), it's easy to end up in a situation where you unintentionally upgrade an application that shouldn't be upgraded.

To put it simply, you can have different/isolated Python environments for each of your projects; you will install required packages for each of your projects into its own isolated environment.

Use `pip` to install virtualenv as well.

    :::bash
    $ sudo pip install virtualenv

Now once virtualenv is installed, run the following commands to create an isolated Python environment for your project.

    :::bash
    $ mkdir my_project_venv
    $ virtualenv --distribute my_project_venv
    # The output will something like:
    New python executable in my_project_venv/bin/python
    Installing distribute.............................................done.
    Installing pip.....................done.

So what's happening here? You created a directory called `my_project_venv` to hold your new isolated Python environment. The `--distribute` tells virtualenv to use new/improved packaging system based on the `distribute` package instead of using the old system based on `setuptools`. All you need to understand right now is that the `--distribute` option will install `pip` automatically within the new virtual environment, so that you don't have to. As your knowledge/experience as a Python developer increase, you will start to better understand these nuts and bolts under the hood.

Now inspect the contents of the `my_project_venv` directory, and you'll see a structure like this:

    :::bash
    # Showing only files/directories relevant to the discussion at hand
    .
    |-- bin
    |   |-- activate  # <-- Activates this virtualenv
    |   |-- pip       # <-- pip specific to this virtualenv
    |   `-- python    # <-- A copy of python interpreter
    `-- lib
        `-- python2.7 # <-- This is where all new packages will go

Activate the virtualenv with the following command:

    :::bash
    $ cd my_project_venv
    $ source bin/activate

After *sourcing* the `activate` script, your prompt should look something like this:
    :::bash
    (my_project_venv)$ # the virtualenv name prepended to the prompt

Now deactivate the virtualenv with the following command:
    :::bash
    (my_project_venv)$ deactivate

Run the following commands to better understand the difference when you use system-wide installation (deactivate the virtualenv first if it's active).

First, let's find out which python/pip executable will be used if I call `python` or `pip` from the terminal.

    :::bash
    $ which python
    /usr/bin/python
    $ which pip
    /usr/local/bin/pip
_Learn about the `which` command at [Wikipedia][whichcommand]._

Now do it again, but activate the virtualenv first and note the differences in the output. On my machine it looks like this:

    :::bash
    $ cd my_project_venv
    $ source bin/activate
    (my_project_venv)$ which python
    /home/mir/my_project_venv/bin/python
    (my_project_venv)$ which pip
    /home/mir/my_project_venv/bin/pip

What `virtualenv` did is make a copy of the Python executable, and create a few utility scripts and a place to install project-specific packages that you'll eventually install/upgrade/remove over the lifetime of the project. It also did some package search path/PYTHONPATH magic to ensure that 1) when you install packages, they are installed inside the currently active virtualenv and not the system-wide Python installation and 2) when imported from code, the packages in the currently active virtualenv will take precedence over the ones installed in system-wide Python installations.

An important thing to note here is that by default, all the packages installed inside the system-wide Python are automatically available to the virtualenv. That means if you installed the `simplejson` package in your system-wide Python installation, it will automatically be available to all the virtualenvs. This behavior can be altered by adding a `--no-site-packages` switch at the time of creation of the virtualenv, like so:

    :::bash
    $ virtualenv my_project_venv --no-site-packages

### virtualenvwrapper

`virtualenvwrapper` is a wrapper around `virtualenv` which provides some really nice utilities to create/activate/manage/destroy virtual environments, which otherwise will be a chore. To install `virtualenvwrapper`, run the following command:

    :::bash
    $ sudo pip install virtualenvwrapper

Once installed, you will need to configure it. Here is how I do it:

    :::bash
    if [ `id -u` != '0' ]; then
      export VIRTUALENV_USE_DISTRIBUTE=1        # <-- Always use pip/distribute
      export WORKON_HOME=$HOME/.virtualenvs       # <-- Where all virtualenvs will be stored
      source /usr/local/bin/virtualenvwrapper.sh
      export PIP_VIRTUALENV_BASE=$WORKON_HOME
      export PIP_RESPECT_VIRTUALENV=true
    fi

Setting `WORKON_HOME` and `source /usr/local/bin/virtualenvwrapper.sh` are the only required pieces of configuration, rest of the configurations are as per my personal preferences.

Add the above configuration at the end of my `~/.bashrc` file and run following command once in your current opened shell windows:

    :::bash
    $ source ~/.bashrc

Same effect can be achieved by closing all open shell windows and tabs and when you open a shell window or a tab again, `~/.bashrc` will be executed, automatically setting the your `virtualenvwrapper` configuration properly.


Now to create/activate/deactivate/delete a virtualenv, you will run following (self explanatory) commands.

    :::bash
    $ mkvirtualenv my_project_venv
    $ workon my_project_venv
    $ deactivate
    $ rmvirtualenv my_project_venv

*Tab-based bash shell command completion also works with virtualenvwrapper.*

Go over to [virtualenvwrapper homepage][virtualenvwrapper] to learn more about available commands and configuration options.

### Basic dependency management with pip and virtualenv

`pip` in combination with `virtualenv` can provide basic dependency management facilities for your project. 

You can use the `pip freeze` command to export the list of currently installed packages. For example, here is the list of Python packages that I use to build this blog:

    :::bash
    $ pip freeze -l 
    Jinja2==2.6
    PyYAML==3.10
    Pygments==1.4
    distribute==0.6.19
    markdown2==1.0.1.19

_Note the `-l` switch. It tells `pip` to export only the packages installed in the currently active virtual environment and exclude the globally installed packages from the list._

You can save this exported list to a file and add it to your version control system.

    :::bash
    $ pip freeze -l  > requirements.txt

`pip` can also install packages from a file containing the output of the `pip freeze` command.

    :::bash
    $ pip install -r requirements.txt


## Other important tools

While we covered the basics of Python versions, VMs and package management, there are other tasks in day-to-day work which require special-purpose tools to accomplish. While I cannot go into every bit of detail for each of the tools, I will try to you give a basic overview.

_Apologies in advance, as most of the tools are specific to web application developers._

### The Editor

There are quite a number of good editors which provide tools for programming in Python. I, personally, am biased towards Vim, but I do not intend to start the _Editor Wars_ - or do I ;).

Some good editors and IDEs (if you like IDEs) that have support for Python programming are Vim/Gvim, Emacs, GEdit for GNOME, Kate for KDE, Scribes, Komodo Edit/IDE from ActiveState, Wing IDE from WingWare, PyCharm from JetBrains, PyDEV for Eclipse. There are others but these seem to be the most popular ones. Choose whichever works best for you.

### Pyflakes: Source checking and linting

Pyflakes is a simple program which checks Python source files for errors by analyzing the text of the file. It checks for syntax and (some) logical errors, imported but unused modules, variables used only once, etc.

You can install it with `pip`:

    :::bash
    $ pip install pyflakes

Call it from the terminal with a Python source file as an argument, like so:

    :::bash
    $ pyflakes filename.py

Pyflakes can be integrated into your editor as well. Here is how it looks on my Vim. Note the _red squiggly lines_.

[![PyFlakes][pyflakes.png]][pyflakes.png]

Ask on [Stack Overflow][stackoverflow] how to add Pyflakes support for the editor of your choice.

[Pyflakes website][pyflakes]

### Requests: HTTP library for human beings 

Requests is a library that makes working with HTTP a breeze. 

Install it with `pip`, like so:

    :::bash
    $ pip install requests

Here's a simple example:

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

Flask is a microframework for Python, based on Werkzeug and Jinja2. 

Install it with `pip`:
   
    :::bash
    $ pip install Flask

Here's a simple example:

    :::python
    from flask import Flask
    app = Flask(__name__)
    
    @app.route("/")
    def hello():
        return "Hello World!"
    
    if __name__ == "__main__":
        app.run()

Run it like so:

    :::bash
    $ python hello.py
     * Running on http://localhost:5000/

[Flask website][flask]

### Django: Full stack framework for web development

Django is a full stack web framework. It provides an ORM, HTTP library, form handling, XSS filtering, and templating, among other things. 

Install with `pip`, like so:

    :::bash
    $ pip install Django

Head over to the [Django website][django] and follow the documentation to learn it. It's quite easy.

### Fabric: Streamline the use of SSH for deployment and/or system admin tasks

Fabric is a Python library and command-line tool for streamlining the use of SSH for application deployment or systems administration tasks.

It provides a basic suite of operations for executing local or remote shell commands (normally or via sudo) and uploading/downloading files, as well as auxiliary functionality such as prompting the running user for input, or aborting execution.

You can install Fabric with `pip`:

    :::bash
    $ pip install fabric

Here's a simple task written with Fabric:

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

PEP 8 is a document that defines coding conventions for the Python code comprising the standard library in the main Python distribution. The sole purpose of the document is to ensure that the python code from everywhere follows same physical layout of the code, naming patterns for variables, class and function names. Make sure you understand it thoroughly and follow it. It will ease you life a lot over time.

[PEP 0008][pep8]

### The Mighty Python Standard Library

Python's standard library is very extensive, offering a wide range of facilities. The library contains built-in modules (written in C) that provide access to system functionality such as file I/O, as well as modules written in Python that provide standardized solutions for many problems that occur in everyday programming. Some of these modules are explicitly designed to encourage and enhance the portability of Python programs by abstracting away platform-specifics into platform-neutral APIs.

Checkout the [Official documentation for the Standard Library][pylib].

## Recommended Reading

David Goodger's [Code Like a Pythonista: Idiomatic Python][idiomaticpy] covers many essential Python idioms and techniques in depth, adding immediately useful tools to your belt. 

Doug Hellmann's excellent series [Python Module of the Week][pymotw]. The focus of the series is building a set of example code for the modules in the Python standard library. 


## Parting thought

What I have covered so far in this tutorial, is just skimming the surface. There is a vast sea of useful tools, libraries and software available for Python for almost every concievable task and which cannot covered in a single go. You will have to discover them yourself, over time.

Python has a great community of really very smart people, who have a very patient and helpful attitude towards new commers to the language. So hang out at IRC channels of your favorite open source projects, follow and ask in mailing lists, talk to people who have experience in implementing small and large systems in Python(and others). Overtime, as your experience/knowledge expands you will become one of them yourself.

I leave you with the **Zen Of Python**. Ponder. Contemplate. Be Enlightened! _Happy Pythoning_

    :::text
    >>> import this
    The Zen of Python, by Tim Peters
    
    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!





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
 [site_module]:http://docs.python.org/library/site.html 
 [idiomaticpy]:http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html 
 [pymotw]:http://www.doughellmann.com/PyMOTW/contents.html 
