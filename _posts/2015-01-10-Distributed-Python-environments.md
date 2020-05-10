---
title: Distributed Python environments
tags:
  - Python
---

With the recent release of [Python 2.7.9](https://www.python.org/downloads/release/python-279/) I was surprised to see some backporting of a few features from the Python 3.x releases. One of the most significant inclusions being the _ensurepip_ module, effectively providing every Python install with a standardised package manager. Arguably Pythons greatest strength is the vast library of packages available (also one of the oft-cited reasons for the slow 3.x uptake, but that's another story) so having the ability to manage these and all their dependencies 'out of the box' is a big deal.

With _pip_ now being included as standard, I though it might be interesting to cover how we deploy and manage Python environments across the art team at work.
(Hint - none of our artists actually install Python on their machines!)

We use version control software (VCS) to deploy and update our toolset, this makes it easy for artists to sync up to new versions and lets us control when new versions are released via a publishing step. So far, so standard... But to use this method for installed software like Python there is some extra setup involved.

#### Example
To distribute a working Python install we have to unpack the installer msi file to a location under VCS...
{% highlight shell-session %}
msiexec /a python-2.7.9.amd64.msi /qb TARGETDIR=J:\Tools\Python279
{% endhighlight %}

Now that it comes bundled with Python, we can easily bootstrap install _pip_ to our tools folder...
{% highlight shell-session %}
J:\Tools\Python279>python.exe -m ensurepip
{% endhighlight %}

Then, use _pip_ to install virtualenv...
{% highlight shell-session %}
J:\Tools\Python279\Scripts>pip.exe install virtualenv

Downloading/unpacking virtualenv
Installing collected packages: virtualenv
Successfully installed virtualenv
Cleaning up...
{% endhighlight %}

Finally we use virtualenv to spawn other Python environments as needed. In this instance we want to create a Python environment that would be accessed from Maxscript in order to do some image processing. So as well as creating the virtual environment, we're also going to use _pip_ to download and install _Pillow_.
{% highlight shell-session %}
J:\Tools\Python279\Scripts>virtualenv.exe --verbose --clear --always-copy J:\Tools\Maxscript\Python
J:\Tools\Maxscript\Python\Scripts\pip.exe install Pillow
{% endhighlight %}

To access this specific Python environment from Maxscript we are altering the `%PATH%` environment variable on 3dsMax startup. Incase there is already a system installation of Python on any artists machine we just pre-pend `%PATH%` with the location of our new environment.
(So `%PATH%` becomes `J:\Tools\Maxscript\Python\Scripts\python.exe;%PATH%`)

Now whenever we call to the command prompt from Maxscript, this virtual Python environment is the one that we are accessing along with all it's installed packages. We can use _pip_ to manage upgrading and installing all the packages we need. If Python is needed anywhere else in the toolchain we can simply create another, isolated environment with a separate set of packages that is completely free from our other environments. Effectivey each tool is running it's own version of Python with a unique set of packages configured around whatever that tool requires.


#### Gotchyas?
Just because there's always one!
If you're trying to use this as a network distributed Python evironment, either using shared network drives or a VCS system... you might run into the drive letter problem. Due to how _virtualenv_ works all of the environments it creates rely on the original install for loading Pythons main packages. Now if your network/version control drive has a different drive letter than some of the people who will be using the virtual environment... then Python will not be able to load it's core packages at startup and will fail.
There are a few possible fixes for this:
You need a static location for the original Python install, that will be valid across anyones machine. One of the easiest ways to do this is just to use a `symlink` from a known location to that of your network/vcs drive. So continuing the previous example, we're going to make a link from our system drive C: to the network drive (this time at K:)...
{% highlight shell-session %}
mklink /j "C:\tools_link" "K:\Tools\"
{% endhighlight %}
When the virtual environment was setup a modified `site.py` file was created. This is where our environment failed when trying to load Pythons core packages. Alongside this file is `orig-prefix.txt` a simple text file to store the path back to the global Python environment. All we have to do is alter this to point to our original Python environment *through* the symlink we just created.
eg `C:\tools_link\Python279`
