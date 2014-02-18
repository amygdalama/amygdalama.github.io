Title: wget Sanity after Installing Homebrew and F*cking Up PATH/PYTHONPATH
Date: 2013-12-02 21:48
Author: Amy Hanlon
Category: Little Eureka Moments
Tags: anaconda, homebrew, osx, python, unix
Slug: homebrew-path-pythonpath

About a week ago, I installed [Homebrew] on my OSX, mostly because I wanted to use Unix's `wget` command (It's like Unix's [accio]! `wget horcrux`. `wget firebolt`.)

Unfortunately, doing so installed a new version of python on my machine and switched the `PATH` and `PYTHONPATH` away from my [Anaconda] version of python (which is the version I use and that houses all of my beloved installed packages). After hours of
struggle, I figured out how to solve this problem.

First, to see if you have this problem, type in the terminal:

	:::bash
	$ type -a python

Which will give you output looking something like this:

	:::bash
	$ type -a python  
	python is /usr/local/bin/python  
	python is //anaconda/bin/python  
	python is /Library/Frameworks/Python.framework/Versions/2.7/bin/python  
	python is //anaconda/bin/python  
	python is //anaconda/bin/python  
	python is /usr/bin/python  
	python is /usr/local/bin/python

This is a hierarchical list of the pythons installed on your machine.
The python that you actually want executed when you type `python` into
the terminal should be at the top of the list. In my situation,
 `//anaconda/bin/python` is my preferred python. To get this version to
the top of the list, you need to edit your `PATH`, which tells your
machine the directory to find `python` when you type it in the terminal.

Similarly, you'll probably need to update your `PYTHONPATH`, which tells
python where to look for modules to import. To see where python is
currently looking for modules, run `python` in your terminal and type the
following commands:

	:::python
	>>> import sys  
	>>> sys.path

As output, python will print out a list of the directories where it
looks for modules:

	:::python
	>>> import sys  
	>>> sys.path  
	['',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/setuptools-1.3-py2.7.egg',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/pip-1.4.1-py2.7.egg',
	'//anaconda/lib/python2.7/site-packages', '/Users/amyhanlon',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python27.zip',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/plat-darwin',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/plat-mac',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/plat-mac/lib-scriptpackages',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/lib-tk',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/lib-old',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/lib-dynload',
	'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages',
	'/Library/Python/2.7/site-packages']

To fix this problem, use the terminal to open the hidden file called
`~/.bash_profile` which exists in your home directory (the `~` symbolizes your home directory). I opened
this file with TextWrangler, but you can replace 'TextWrangler' in the
command with whichever text editing program you use (the default OSX
text editor is TextEdit).

	:::bash
	$ open -a TextWrangler ~/.bash_profile

The file contents should look something like this:

	:::bash
	# Setting `PATH` for Python 2.7  
	# The original version is saved in .bash_profile.pysave  
	PATH="/Library/Frameworks/Python.framework/Versions/2.7/bin:${PATH}"  
	export `PATH`  
	# added by Anaconda 1.7.0 installer  
	export PATH="//anaconda/bin:$PATH"  
	export PATH=/usr/local/bin:$PATH  
	export PYTHONPATH="/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages:$PYTHONPATH"

The last PATH= and PYTHONPATH= lines should be directed to the Anaconda
location of python, and in this case they aren't. To fix this, comment
out or delete lines that incorrectly assign the `PATH` and `PYTHONPATH` variables, and
add lines that assign the correct variables. After making edits, my file looks like this:

	:::bash
	# Setting PATH for Python 2.7  
	# The original version is saved in .bash_profile.pysave  
	#
	PATH="/Library/Frameworks/Python.framework/Versions/2.7/bin:${PATH}"  
	PATH="//anaconda/bin:${PATH}"  
	export `PATH`  
	# added by Anaconda 1.7.0 installer  
	export PATH="//anaconda/bin:$PATH"  
	# export PATH=/usr/local/bin:$PATH  
	# export
	PYTHONPATH="/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages:$PYTHONPATH"  
	export PYTHONPATH="//anaconda/lib/python2.7/site-packages:$PYTHONPATH"

Save the file. Then, in the terminal (after exiting python), source the
file, and see if you've fixed the `PATH` and `PYTHONPATH`:

	:::bash
	$ source ~/.bash_profile  
	$ type -a python
	python is //anaconda/bin/python  
	python is //anaconda/bin/python  
	python is /usr/local/bin/python  
	python is //anaconda/bin/python  
	python is /Library/Frameworks/Python.framework/Versions/2.7/bin/python  
	python is //anaconda/bin/python  
	python is //anaconda/bin/python  
	python is /usr/bin/python  
	python is /usr/local/bin/python

Now you should see the Anaconda python (or your preferable python) at
the top of the list in the output.


Check your `PYTHONPATH` in the python interpreter by using the same method as before:

	:::python
	>>> import sys  
	>>> sys.path  
	['', '//anaconda/lib/python2.7/site-packages/gspread-0.1.0-py2.7.egg',
	'//anaconda/lib/python2.7/site-packages/fuzzywuzzy-0.2-py2.7.egg',
	'//anaconda/lib/python2.7/site-packages/inflect-0.2.4-py2.7.egg',
	'//anaconda/lib/python2.7/site-packages/beautifulsoup4-4.3.2-py2.7.egg',
	'//anaconda/lib/python2.7/site-packages/pyicloud-0.3.0-py2.7.egg',
	'//anaconda/lib/python2.7/site-packages/foursquare-20130707-py2.7.egg',
	'//anaconda/lib/python2.7/site-packages/poster-0.8.1-py2.7.egg',
	'//anaconda/lib/python2.7/site-packages', '/Users/amyhanlon',
	'//anaconda/lib/python27.zip', '//anaconda/lib/python2.7',
	'//anaconda/lib/python2.7/plat-darwin',
	'//anaconda/lib/python2.7/plat-mac',
	'//anaconda/lib/python2.7/plat-mac/lib-scriptpackages',
	'//anaconda/lib/python2.7/lib-tk', '//anaconda/lib/python2.7/lib-old',
	'//anaconda/lib/python2.7/lib-dynload',
	'//anaconda/lib/python2.7/site-packages/PIL',
	'//anaconda/lib/python2.7/site-packages/setuptools-0.6c11-py2.7.egg-info']

Now the terminal is using my Anaconda version of python and is looking
in the Anaconda library for my packages! Viola!

  [Homebrew]: http://brew.sh/
  [accio]: http://en.wikipedia.org/wiki/List_of_spells_in_Harry_Potter
  [Anaconda]: https://store.continuum.io/cshop/anaconda/
