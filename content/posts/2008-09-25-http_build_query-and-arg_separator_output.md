---
tags:
- PHP
date: "2008-09-25T00:00:00Z"
description: If you've run into problems with PHP's http_build_query function, setting
  arg_separator.output may help.
keywords: arg_separator.output, http_build_query, php
title: http_build_query() and arg_separator.output
url: /php/http_build_query-and-arg_separator_output.html
---
The other day I was using [http_build_query](http://php.net/http_build_query) to generate URLs to use with the [Yahoo
Geocoding API](http://developer.yahoo.com/maps/rest/V1/geocode.html) and kept getting back a "400 Bad Request" error back
from Yahoo. The URLs were being echoed and looked fine, I could
copy the URL and paste it in the browser address bar and get the results I was looking for.

When I finally viewed the source I saw that the urls
were being created with &amp;amp; as the separator for the variables in the
query string instead of &amp;. That explained why the url looked fine when
it was echo'ed but didn't work when executed.

This is where [arg_separator.output](http://php.net/manual/en/ini.core.php#ini.arg-separator.output) comes in. This setting in php.ini determines
what character(s) are used when generating URLs using the built in PHP
functions. So to get my code working, I just needed to change the value
to just an &amp;: ```php<?php ini_set('arg_separator.output','&'); ?>```

I just wish it hadn't taken me so long to figure that out!
