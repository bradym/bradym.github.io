---
layout      : post
category    : Windows
title       : "Testing in multiple versions of IE"
keywords    : "ie, testing sites in ie, multiple versions of ie"
description : "Internet Explorer is the bane of every web developer's existance. What makes it worse is how difficult Microsoft makes it to test sites in different versions of IE."
date        : 2008-08-27
permalink   : /windows/testing-in-multiple-versions-of-ie.html
---
One question that I get all the time is how to test a website in both
IE6 and IE7 on the same computer. Here are some options:

### Virtual Machine

Because the code for Internet Explorer is so tightly integrated into
Windows, the only way to run the "pure / authentic" version of IE6 on
the same machine as IE7 is by using a Virtual Machine. Microsoft has
made this easy by providing a [Virtual PC image for this
purpose](http://www.microsoft.com/downloads/details.aspx?FamilyID=21eabb90-958f-4b64-b5f1-73d0a413c8ef&displaylang=en).
Virtual PC is also available for [free
download](http://www.microsoft.com/windows/products/winfamily/virtualpc/default.mspx),
so this is a great option. There are two downsides to this option, the
VPC image is over 400MB in size, and the image periodically expires
requiring a new download every 6 months or so.

If you have a copy of Windows XP sitting around you could use [Virtual
PC](http://www.microsoft.com/windows/products/winfamily/virtualpc/default.mspx),
[VMWare](http://vmware.com/), [VirtualBox](http://virtualbox.org/) or
any other virtualization software to create your own non-expiring image
for testing.

### IE6 Standalone

The [Evolt Browser Archive](http://browsers.evolt.org/) includes a
[standalone version of
IE6](http://browsers.evolt.org/download.php?/ie/32bit/standalone/ie6eolas_nt.zip).
Simply download the zip archive, unzip and run iexplore.exe. Simplifiy
your life by creating a shortcut in your start menu or on your desktop.
This is my preferred method, mostly due to the simplicity, and has
worked very well for me.

### Multiple IE

Want to test more than just IE6? [Multiple
IE](http://tredosoft.com/Multiple_IE) installs IE3 - IE6 standalone
versions. It's been a long time since I've cared about how something
looked in anything earlier than IE6, but if you have a reason to test a
site on an earlier version of IE, this is a good way to go.

### IE7 Standalone

The above options assume that you've already installed IE7. The makers
of Multiple IE also have an installer for [IE7
standalone](http://tredosoft.com/IE7_standalone) (but it's not
integrated into Multiple IE). The idea here is that ou would run IE6
natively, with IE7 in standalone mode. I've never sucessfully used this
option, but others have found it useful.

### Gotchas

Running a standalone version of Internet Explorer ***is not the same***
as running the native, built-into-windows version.

Due to IE's reliance on DLLs and registrty settings, things can get
messy in certain situations. Take a look at [Taming Your Multiple IE
Standalones](http://www.positioniseverything.net/articles/multiIE.html)
for more information on these possible snags. In short, they recommend
using Multiple IE, as the registry hacks and DLLs are all included.
