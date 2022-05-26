---
tags:
- Windows
date: "2008-04-28T00:00:00Z"
description: Compared to *nix based systems, the windows command prompt is a joke.
  If you're stuck on windows, here are some tips to make the command line experience
  a little less painful.
keywords: windows command line, console, cygwin, gnuwin32
title: Making the Windows Command Prompt Usable
url: /windows/making-the-windows-command-prompt-usable.html
---
If you've used an interactive shell on Linux or OS X you know what a
good command prompt is like. To go from that to the command prompt in
Windows XP is painful, since I'm stuck on Windows at work, I had to do
something about the command prompt to make it more bearable.

### Install Console

[Console](http://sourceforge.net/projects/console "Console") is an [Open
Source](http://www.opensource.org/) command prompt window enhancement.
If you're familiar with [Konsole](http://konsole.kde.org/) from
[Kde](http://kde.org), Console has a similar feature set.

Console allows you much greater control over the display and setup of
the command prompt including resizing the console, specifying background
and font colors, specifying a startup directory, using tabs, etc.

There's no installer with Console, just extract the zip file and run
console.exe. I've created shortcuts in my taskbar and on the desktop for
easy access. Once you've launched Consoe, be sure to spend some time
playing around with the settings, that's where all the power is!

### Install Cygwin or GNUWin32

Now that we have a decent console, it's time to get some \*nix goodness!
There are two great options for getting ports of \*nix tools working on
Windows: [Cygwin](http://www.cygwin.com) and
[GnuWin32](http://gnuwin32.sourceforge.net/).

Cygwin provides a more \*nix like environment, as well as a larger
number of tools overall. Cygwin includes a batch file to launch a
command prompt with bash as the interpreter. With Cygwin you can also
install an X server and run programs that require an X server, though if
you're trying to do that it'd be much easier to install
[VirtualBox](http://www.virtualbox.org/) or [VMWare
Player](http://www.vmware.com/products/player/) and run a virtual
instance of your favorite \*nix distro.

GnuWin provides Windows ports of many \*nix tools, but does not try to
imitate the \*nix filesystem like Cygwin does. If you're only looking
for specific tools it may be easier to get them from GnuWin32.

I'm currently using Cygwin as it's a standard at work.

### Using \*nix Tools from the Windows Command Line

Now that we have our command line and our \*nix tools, all that's left
is to connect the two by adding the folder with the binaries to your
Windows path. For Cygwin this is c:\\cygwin\\bin by default.

1.  Right click on "My Computer" on your desktop (or in windows
    explorer)
2.  Select "Properties"
3.  Click the "Advanced" tab
4.  Click on "Environment Variables"
5.  Look at the top list of variables,
    -   If there is not a variable named "Path"
        1.  Click on "New"
        2.  Enter "Path" in the "Variable name" field
        3.  Enter "C:\\cygwin\\bin" in the "Variable value" field

    -   If there is a variable named "Path"
        1.  Click "Edit"
        2.  Click in the "Variable value" box and go to the end of the
            field
        3.  Add a semi-colon ( ; )
        4.  Enter "C:\\cygwin\\bin"
6.  Click OK 3 times and you should be ready to go

Open Console and enjoy your much improved windows command prompt experience!
