---
tags:
- PHP
date: "2009-04-19T00:00:00Z"
description: Looking for a simple way to share code with fellow developers on a local
  network? This post shows one way to do just that using PHP.
keywords: php, syntax highlighting, team development, debugging
title: Simple PHP Syntax Highlighting
url: /php/simple-php-syntax-highlighting.html
---
One benefit to working as part of a development team is that you can ask your fellow developers to look at your code and ask for help. Sending code back and forth over email can be frustrating and even counter-productive. IMO, the ideal way to share code is by simply sending a URL and letting the other developer view it in their browser with syntax highlighting.

Here's one way that does not require any external libraries and is very easy to setup:

## Add this line to .htaccess
```apache
RewriteRule ^(.*)\.phps$ viewsource.php?file=$1.php [QSA,L]
```

This rule sends all requests for files ending in .phps to the viewsource.php file with the path as an argument.
## Put the below script in the document root of your development server
```php
<?php
// This script should *NEVER* be used on a production server!
// By default, it will only run on localhost. If you want to run this script on
// a staging server add the IP address of that server to $allowed_servers.
$allowed_servers = array('127.0.0.1');

// Set $base_dir to the root of your web directory.
$base_dir = $_SERVER['DOCUMENT_ROOT'];

// Make sure this script is running on an allowed server.
if(!in_array($_SERVER['SERVER_ADDR'], $allowed_servers)){
    die('Do not run this script in production!');
}

// Exit if no file was specified
if(empty($_GET['file'])){
    die('No file specified.');
}

// Ensure the requested file is inside $base_dir and that it exists
$real_path = realpath($base_dir.$_GET['file']);

if(strpos($real_path, $base_dir) === false || $real_path === false){
    die('Access denied or file not found.');
}

?>
<html>
<head>
<title><?= $real_path ?></title>
<style type="text/css">
    .num_margin {
        text-align: right;
        margin-right: 6pt;
        padding-right: 6pt;
        border-right: 1px solid gray;
    }
    .num_margin a {
        color: gray;
        font-size: 13px;
        font-family: monospace;
    }
    body { margin: 0px; margin-left: 5px; }
    div { float: left; }
</style>

</head>
<body>
<?php
$lines = '';
$lines_array = range(1, count(file($real_path)));
foreach($lines_array as $line){
    $lines .="<a href=\&quot;#$line\&quot; name=\&quot;$line\&quot;>$line</a><br />";
}
$content = highlight_file($real_path, true);
echo "<div><div class=\"num_margin\">$lines</div><div>$content</div></div>";
?>
</body>
</html>
```

Now when you visit a link like: `http://localhost/info.phps` it will show the source of the file.

Note:  This script should **never** be used in production!
