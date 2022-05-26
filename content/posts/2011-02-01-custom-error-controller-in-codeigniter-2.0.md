---
tags:
- PHP
date: "2011-02-01T00:00:00Z"
description: One long-awaited feature of CodeIgniter 2.0 is using a controller for
  the 404 page. Unfortunately it doesn't quite work in all situations. Here's one
  appraoch to fix the behavior.
keywords: CodeIgniter, php, custom error handler
title: Custom Error Controller in CodeIgniter 2.0
url: /php/custom-error-controller-in-codeigniter-2-0.html
---
[CodeIgniter 2.0](http://codeigniter.com) was released a few days ago,
and one of the (much overdue) features I'm looking forward to using is
the ability to define a custom 404 controller. This is useful for
handling requests to non-existant controller methods by specifying a
controller to call instead. Here's how to get it setup:

1.  Get the latest CodeIgniter 2.0 code from
2.  Create an error controller with an error\_404 method.
3.  Modify application/config/routes.php by setting
```php
    <?php
    $route['404_override']  = 'error/error_404';
    ?>
```

You should now have a functional error controller for 404s when
triggered by a request for non-existent controller methods. But what if
you want to trigger 404s from your code and use the error controller?

Something like this, perhaps?

```php
<?php
// Get an article from the database, show a 404 page if the requested article was not found.
$article = get_content($this->uri->uri_string();
if(empty($article){
    show_404($this->uri->uri_string());
}
?>
```
Unfortunately, this will still use the built-in non-controller error
message. I got to playing around and discovered that (at least in CI
2.0, maybe 1.7.2) you can do this:
```php
<?php
$article = get_content($this->uri->uri_string();
if(empty($article){
    include(APPPATH.'controllers/error.php');
    $error = new Error();
    $error->error_404();
}
?>
```
Of course you could do the same thing for other error types (403, 500,
etc) by creating new methods in the Error controller and calling the
correct method. It could even be simplified more by creating a helper
with a function such as this one:

```php
<?php
function custom_error($status_code = 404){
    $method_name = 'error_'.$status_code;
    include(APPPATH.'controllers/error.php');
    $error = new Error();
    $error->$method_name();
}
?>
```
