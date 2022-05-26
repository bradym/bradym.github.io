---
tags:
- PHP
date: "2011-05-19T00:00:00Z"
description: Do you understand what the different php.ini settings around error messages
  are and how to use them? Here's a description of some of the most important settings
  related to errors and how to use them effectively.
keywords: php, error_reporting, display_errors, log_errors, error_log, track_errors,
  error suppression operator
title: Understanding error settings in PHP
url: "/php/understanding-error-settings.html"
---
Based on the number of questions I see on sites like
[stackoverflow](http://www.stackoverflow.com) around PHP errors and
things I've seen during code reviews, it seems the different error
settings in PHP are a bit of a mystery to many PHP developers. In my
opinion having a good understanding of the different settings around
error messages is vital. If you don't know how to set the error
reporting level, how errors are logged or how and when to show errors on
the screen it makes debugging much more difficult.

Some of the most important settings around errors include:

### [error\_reporting](http://www.php.net/manual/en/errorfunc.configuration.php#ini.error-reporting)

The `error_reporting` setting defines what kinds of messages are
reported. See
[http://www.php.net/manual/en/errorfunc.constants.php](http://www.php.net/manual/en/errorfunc.constants.php)
for the different error levels. It is possible to disable error messages
completely by setting `error_reporting` to `0`, but I can't think of a good
reason to do this. _Ever_.

I recommend always setting the error level to `E_ALL` in production, and
`E_ALL | E_STRICT` in dev and test. This will ensure that you're aware
of all types of errors, and even notified of improvements that could be
made to your code.

### [display\_errors](http://www.php.net/manual/en/errorfunc.configuration.php#ini.display-errors)

Display errors determines if errors are printed to the screen. Because
error messages can give detailed information about your site, server
setup, operating system, and more, display errors should always be
turned off in production.

I recommend always enabling `display_errors` in dev and test. Having the
error messages stare you in the face when you're working on dev and test
can make it much easier to identify and resolve issues. Along with this,
I recommend installing [Xdebug](http://xdebug.org) in dev and test.
Xdebug provides great formatting of error messages as well as stack
traces so it's easier to identify exactly where an error occurred.

### [log\_errors](http://www.php.net/manual/en/errorfunc.configuration.php#ini.log-errors)

This setting enables error logging. It should always be enabled,
especially in production. Reviewing error logs on a regular basis is a
great way to identify code issues that you may not be aware of, as well
as troubleshooting known issues.

### [error\_log](http://www.php.net/manual/en/errorfunc.configuration.php#ini.error-log)

This setting is used to specify where to log PHP errors. By default when
using PHP with Apache, PHP errors will be sent to the apache error log.
Separating the two log files can be useful, but is not required. The
benefit of separating the two is that you don't have to wade through
404s or other Apache level errors that may not be related to your code.
If you do separate the two, I suggest reviewing each of them regularly
as the Apache logs can also be quite helpful in identifying issues with
your site.

### [track\_errors](http://www.php.net/manual/en/errorfunc.configuration.php#ini.track-errors)

If you enable this setting the last error message will be stored in the
`$php_errormsg` variable. This is probably one of the lesser used error
settings, but it can be very useful in certain situations.

PHP also supports an [error suppression
operator](http://php.net/manual/en/language.operators.errorcontrol.php),
the`@` sign. When used, any errors that result from the following
expression will not be reported. Read that again. I didn't say it fixes
errors, or eliminates errors, or makes your code work any differently. I
said PHP **won't tell you** that an error occurred. The error
suppression operator should only be used in very rare, and very specific
cases.

If you decide to use the error suppression operator, you'll need to be
extra careful about checking for error conditions around that code. Even
though the error won't be logged or displayed to the screen, the error
message will be available in `$php_errormsg` if you enable the
`track_errors` setting. Using this setting along with the error
suppression error can improve your code, but it should not be an excuse
to use the error suppression operator.

Anytime you use the error suppression operator, add a comment explaining
why you're using it. If you have to explain to everyone who reads your
code in the future why you did something, you're more likely to explore
better options first. For those cases where it really is the right way
to solve a problem (despite my warnings against using it, there's
occasionally a very good reason to use it) having that documentation
will save future developers the effort of figuring out why it's being
used.
