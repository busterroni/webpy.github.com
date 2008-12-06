---
layout: default
title: Download web.py
---

# Download web.py


## Get the latest stable version

The easiest way to install web.py is using `easy_install`:

    $ easy_install web.py

Or by downloading the sources.

    $ wet http://webpy.org/static/web.py-0.3.tar.gz
    $ tar xvzf web.py-0.3.tar.gz
    $ cd webpy
    $ sudo python setup.py install

If you don't want to install web.py system-wide (or if you want to bundle web.py with your application):

    $ cd your-app-dir
    $ wet http://webpy.org/static/web.py-0.3.tar.gz
    $ tar xvzf web.py-0.3.tar.gz
    $ ln -s webpy/web .
   
If you are on Ubuntu Linux or Debian, you can install web.py using `apt-get`. But you may not get the latest as debian/ubuntu release cycles are different from web.py.

    $ sudo apt-get install python-webpy

## Get the latest development version

    $ git clone git://github.com/webpy/webpy.git
