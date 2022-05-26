---
tags:
- PHP
date: "2009-02-25T00:00:00Z"
description: Using Xdebug on OS X without installing a 3rd party version of Apache
  is possible, here's how.
keywords: xdebug os x, php, xdebug
title: Setup Xdebug on OS X Leopard
url: /php/setup-xdebug-on-os-x.html
---
**Note:** If you're using Leopard, the directions below are for you. If
you're using Snow Leopard, just follow the directions provided at
[xdebug.org](http://xdebug.org/docs/install).

Most of the blog posts I've seen about getting
[Xdebug](http://xdebug.org/) working on OS X insist that you must use
[MAMP](http://www.mamp.info/) or [XAMPP](https://www.apachefriends.org/index.html) to do so. But
since [PHP](http://php.net) and [Apache](http://httpd.apache.org) come
included with OS X, it seems like overkill to use one of those packages.

Contrary to popular belief, it is possible to get Xdebug working with
the OS X standard Apache/PHP installation. Here's how I did it:

1. [Download Xdebug source](http://xdebug.org/download.php)
2. Extract the xdebug tarball
3. cd into the xdebug directory

```bash
phpize

MACOSX_DEPLOYMENT_TARGET=10.5 CFLAGS="-arch ppc -arch ppc64 -arch i386 -arch x86\_64 -g -Os -pipe -no-cpp-precomp" CCFLAGS="-arch ppc -arch ppc64 -arch i386 -arch x86\_64 -g -Os -pipe" CXXFLAGS="-arch ppc -arch ppc64 -arch i386 -arch x86\_64 -g -Os -pipe" LDFLAGS="-arch ppc -arch ppc64 -arch i386 -arch x86\_64 -bind\_at\_load" ./configure --enable-xdebug```

make

cp modules/xdebug.so /usr/lib/php/extensions/no-debug-non-zts-20060613 # note: your path may be slightly different
```

4. add the following line to php.ini: `zend_extension="/usr/lib/php/extensions/no-debug-non-zts-20060613/xdebug.so"`
5. sudo apachectl restart
6. Write a PHP page that calls `phpinfo();` Load it in a browser and look for the info on the xdebug module. If you see it, you have been successful!

This is simply a modified version of the README file that comes with the
xdebug source. The most important tweak is step 5, which sets the
correct flags for the resulting module to work with Apache as it is
compiled in OS X.

Note: If you're currently using another debugger, you will need to
disable it before Xdebug will work.
