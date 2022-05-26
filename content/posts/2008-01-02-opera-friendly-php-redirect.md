---
tags:
- PHP
date: "2008-01-02T00:00:00Z"
description: Having trouble getting a redirect to work in Opera? This might help.
keywords: opera browser, redirect, php
title: Opera friendly PHP redirect
url: /php/opera-friendly-php-redirect.html
---
Opera can't handle a redirect to a URL that ends in an anchor. I found this out trying to use the PHP `header()` function.

From what little I was able to find using Google on the topic, it appears that Opera won't redirect a user to the page they just came from, it must be a different URI.

To get around this in php, here's what I did:

```php
<?php
header('Location: file.php?value1=12&value2=43&r='.rand().'#anchorName');
?>
```

Having the random number from rand() makes the page unique, and the redirect works as expected.
