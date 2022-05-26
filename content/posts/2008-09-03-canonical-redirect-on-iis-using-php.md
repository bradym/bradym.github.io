---
tags:
- PHP
date: "2008-09-03T00:00:00Z"
description: Using a Windows hosting account where you don't have the ability to modify
  IIS configurations? Here's one method for setting up a canonical redirect.
keywords: canonical redirect, iis, php, windows shared hosting
title: Canonical redirect on IIS using PHP
url: /php/canonical-redirect-iis.html
---
According to the [seo
experts](http://www.mattcutts.com/blog/seo-advice-url-canonicalization/)
of the world, it is good practice to force your website to always use
the same domain. So, instead of having your site work at both
www.example.com and example.com, choose one. Once you've chosen which
form of your site to use, it's time to setup 301 redirects to enforce
that.

If you're using Apache, it's very easy, just add a quick mod\_rewrite
rule. You can [remove the
www](http://www.askapache.com/htaccess/mod_rewrite-tips-and-tricks.html#require-no-www-in-htaccess)
or [force the use of
www](http://www.askapache.com/htaccess/mod_rewrite-tips-and-tricks.html#require-the-www-in-htaccess).
But since IIS doesn't use .htaccess files, it takes a little more
effort.

### Create a php.ini with the following content and upload it to your site root

{{< highlight ini >}}
cgi.force_redirect = 0
auto_prepend_file = C:\php_includes\canonical.php
```

Keep in mind that you'll need to change the file path to reflect the
setup of your server.

### Upload canonical.php with the following contents

```php
<?php
// Any directory index filenames should go in here with a leading /
// This is removed when doing the redirect in order to maintain consistency,
// without this, when visiting example.com you would be redirected to www.example.com/index.htm
$index_files = array('/index.htm','/index.html','/index.php');

// The rewrite only kicks in if the host does not begin with www. and the code is not running
// on localhost. When developing a site I setup dev.example.com on my dev machine and use
// the same code as is on the live server, so I don't want to be redirected if I'm
// accessing the code locally.
if(substr($_SERVER['HTTP_HOST'],0,4) !== 'www.' && $_SERVER['LOCAL_ADDR'] !== '127.0.0.1'){
    $url = 'http://www.'.$_SERVER['HTTP_HOST'].str_replace($index_files, '', $_SERVER['PATH_INFO']);
    header ('HTTP/1.1 301 Moved Permanently');
    header("Location: $url");
    exit();
}
?>
```

### Notes:

-   This method assumes that you don't have access to modify the server
    settings, ie: shared windows hosting. If you are able to modify how
    IIS is setup I'm sure there's a better way to do this.
-   When PHP is run as a cgi, you can overwrite the php.ini settings by
    putting a php.ini in the site root. So if your windows host is not
    running php as a cgi, this won't work. I think that php is always
    run as a cgi on IIS, but I could be wrong.
-   This will only work if the site is using PHP - if you have a
    completely static site this won't help you. In order to use this
    with a static site, you'd have to change the server settings to
    treat your html files as php files.

