---
layout      : post
category    : PHP
title       : "Creating iCalendar (ics) files with PHP"
keywords    : "ics, ical, icalendar, php"
description : "Trying to create an ics file using PHP? Here's one way to do it."
date        : 2009-08-15
permalink   : /php/creating-icalendar-ics-files-with-php.html
---
This post was originally written several years ago and included my own
attempt at building a very simple ical creator in PHP. I've been meaning
to update this blog with some better code and information for quite some
time. I finally found what I consider to be a very good class for
creating iCalendar files using PHP:
[iCalcreator](http://www.kigkonsult.se/iCalcreator/index.php). Along
with creating iCalendar files, iCalcreator can also [parse and merge
iCalendar
files](http://www.kigkonsult.se/iCalcreator/docs/using.html#parse_merge),
and spit out a modified version. There are lots of [examples available
for download](http://www.kigkonsult.se/downloads/index.php?#iCalcreator)
on their site, as well as supporting utilities.

The [iCalcreator
documentation](http://www.kigkonsult.se/iCalcreator/docs/using.html) is
excellent. The examples are easy to follow, and they clearly document
how to use their class. Unfortunately, the iCalendar specification is
complicated enough that you'll still need to read rfc2445 to really take
advantage of all of the options available to you. Some good iCalendar
reference sites include:

-   [iCalendar - Wikipedia](http://en.wikipedia.org/wiki/ICalendar)
-   [iCalendar Specification
    Excerpts](http://www.kanzaki.com/docs/ical/)
-   [Entire iCalendar Specification
    (rfc2445)](http://www.rfc-editor.org/rfc/rfc2445.txt)

Since I haven't actually built anything using iCalcreator yet (other
than some quick test scripts earlier today) I can't really answer any
questions about its usage. The good news is they have a [forum on
sourceforge](http://sourceforge.net/forum/?group_id=174828) for
questions. I'd love to hear about your experiences using iCalcreator, so
feel free to leave your comments below.
