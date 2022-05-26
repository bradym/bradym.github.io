---
tags:
- MySQL
date: "2008-07-06T00:00:00Z"
description: Using "ON DUPLICATE KEY UPDATE" can be very useful when migrating data
  or logging to a MySQL database.
keywords: mysql, on duplicate key update
title: MySQL - on duplicate key update
url: /mysql/on-duplicate-key-update.html
---
If you've ever built an application that interacts with a database and
allows users to edit data, or one that migrates data from one system to
another you've probably written code like this:

```php
<?php
// $id would have been set based on some other operation above.
// I'm setting it here for clarity.
$id = 1;
$result = mysql_query("SELECT id FROM table_name WHERE id = $id");
if(mysql_num_rows() == 0){
    // Zero results, the record doesn't exist, add it.
    mysql_query("INSERT INTO table_name (id,a,b) VALUES ($id,2,3)");
} else{
    // The record exists, update it.
    mysql_query("UPDATE table_name SET a = 1, b = 2 WHERE id = $id");
}
?>
```

Well, thanks to the ON DUPLICATE KEY UPDATE option in MySQL this can be
condensed to the following:

```php
<?php
$result = mysql_query("INSERT INTO table_name SET id = $id, a = 2, b = 3
                        ON DUPLICATE KEY UPDATE
                        a = 2, c = 3");
?>
```
### Notes:

-   You don't have to update every field on update. If you only need one
    row updated if the row already exists, specify only that row in the
    UPDATE portion of the query.
-   I prefer the above syntax (using set and writing field = value for
    each field) but as shown in the [MySQL
    manual](http://dev.mysql.com/doc/refman/5.0/en/insert-on-duplicate.html),
    it works with the (field1,field2,field3) VALUES
    (value1,value2,value3) version as well.
-   ***This method does not work perfectly in all situations! Be sure to
    test on your dev server before using in production!***

