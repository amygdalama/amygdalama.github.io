Title: wget sanity part 2: I'm an idiot and decided to switch to a brewed python (and reinstall the scientific stack) at 10pm
Date: 2013-12-27 01:08
Author: amygdalama
Category: Little Eureka Moments
Tags: homebrew, matplotlib, osx, python
Slug: wget-sanity-part-2-im-an-idiot-and-decided-to-switch-to-a-brewed-python-and-reinstall-the-scientific-stack-at-10pm

For the past four months I've been using Anaconda's Python distribution
on my Macbook, which has been great (except for [Part 1][] of what has
now become a series), until I wanted to play with virtualenv. Apparently
Anaconda does not work well with virtualenv and suggests using its own
conda virtual environments and I don't take well to The Man telling me I
can't do something I want to do. So of course my reaction was to
`rm -rf /anaconda`.

Now I'm building my Python stack back from the ground up, and I've
decided to try out a [brewed][] Python. I've been following [these][]
very elegant instructions to set up numpy, scipy, etc, on OSX, and it
worked flawlessly! Until matplotlib! I had all of the dependencies
(freetype, which I installed via Homebrew, zlib, and libpng), but I kept
getting thrown this error:

[code gutter="false"]fatal error: 'freetype/config/ftheader.h' file not
found

\#include \<freetype/config/ftheader.h\>[/code]

I finally found [this][] StackOverflow discussion with the suggestion to
type this into the terminal (for brewed python):

[code gutter="false"]\$ ln -s /usr/local/include/freetype2/
/usr/include/freetype[/code]

And then `sudo pip install matplotlib` worked! I don't rigorously
understand this magic, but based on my research this creates a 'symbolic
link' from `/usr/include/freetype` to `/usr/local/include/freetype2`
(the brewed freetype). I'm guessing that matplotlib by default was
looking for freetype in `/usr/include/freetype`, but it wasn't there
since Homebrew installs everything in `/usr/local`. So, creating the
symbolic link allowed matplotlib to find freetype. In moments like these
I'm like 'Yer a wizard, Harry.'

  [Part 1]: http://mathamy.com/2013/12/02/homebrew-path-pythonpath/
  [brewed]: https://github.com/Homebrew/homebrew/wiki/Homebrew-and-Python
  [these]: http://www.lowindata.com/2013/installing-scientific-python-on-mac-os-x/
  [this]: http://stackoverflow.com/questions/1477144/compile-matplotlib-for-python-on-snow-leopard
