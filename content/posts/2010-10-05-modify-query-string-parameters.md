---
tags:
- PHP
date: "2010-10-05T00:00:00Z"
description: How to modify the query string in a given url. Can be very useful when
  implementing pagination.
keywords: php, parse_str, parse_url, http_build_query, simple solution for simple
  problem
title: Modify Query String Parameters
url: /php/modify-query-string-parameters.html
---
I just saw [this
article](http://www.phpsnippets.info/easily-modify-url-parameters) in my
RSS feeds, and was amazed at the complexity of the solution to a simple
problem. The goal was to change query strings in a URL to the new values
passed in as an associative array. If the URL didn't have a query
string, add one using the array. Also, query strings in the original URL
not in the array shouldn't be modified.

The provided function uses regular expression matching, multiple foreach
loops, and really makes the problem more complex than it needs to be.

```php
<?php
$url = modify_url(array('p' => 4, 'show' => 'column'), 'http://www.example.com/page.php?p=5&show=list&style=2');
?>
```

`$url` is now http://www.example.com/page.php?p=4&show=column&style=2

So here's my version:
```php
<?php
function modify_url($mod, $url = FALSE){
    // If $url wasn't passed in, use the current url
    if($url == FALSE){
        $scheme = $_SERVER['SERVER_PORT'] == 80 ? 'http' : 'https';
        $url = $scheme.'://'.$_SERVER['HTTP_HOST'].$_SERVER['REQUEST_URI'];
    }

    // Parse the url into pieces
    $url_array = parse_url($url);

    // The original URL had a query string, modify it.
    if(!empty($url_array['query'])){
        parse_str($url_array['query'], $query_array);
        foreach ($mod as $key => $value) {
            if(!empty($query_array[$key])){
                $query_array[$key] = $value;
            }
        }
    }

    // The original URL didn't have a query string, add it.
    else{
        $query_array = $mod;
    }

    return $url_array['scheme'].'://'.$url_array['host'].'/'.$url_array['path'].'?'.http_build_query($query_array);
}
?>
```
I'm guessing the original author is just not familiar with
[parse\_str](http://php.net/parse_str),
[parse\_url](http://php.net/parse_url) and
[http\_build\_query](http://php.net/http_build_query).

Moral of the story: regularly read through the PHP manual to see what's
available. You'll save yourself time in the long run, and avoid
re-inventing the wheel so often.

(Yes, I'm fully aware that I've just become that guy in XKCD.)

![Duty Calls](http://imgs.xkcd.com/comics/duty_calls.png "What do you want me to do?  LEAVE?  Then they'll keep being wrong!")
