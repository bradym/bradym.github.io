---
tags:
- PHP
date: "2009-02-24T00:00:00Z"
description: Storing the latitude and longitude of addresses in your database speeds
  up the generation of map-based mashups. Here's a solution using the Yahoo! Geocoding
  API.
keywords: Yahoo! Geocoding API, latitude, longitude, php
title: Geocoding with PHP
url: /php/geocoding-with-php.html
---
In my [last
post](http://bradym.net/php/http_build_query-and-arg_separator_output) I
mentioned geocoding using the [Yahoo Geocoding
API](http://developer.yahoo.com/maps/rest/V1/geocode.html), so I thought
I'd post some code to do exactly that.

For the purposes of this example, assume that we have a very simple
MySQL database table with this structure:

{{< highlight mysql >}}
CREATE TABLE addresses (
        id int(11) NOT NULL auto_increment,
        street varchar(255) default NULL,
        city varchar(255) default NULL,
        state varchar(2) default NULL,
        zip varchar(9) default NULL,
        lat float(10,6) default NULL,
        lon float(10,6) default NULL,
        PRIMARY KEY  (id)
)
```

Here's the script:

```php
<?php
// Geocode addresses using the Yahoo Geocoding API

// Turning on track_errors stores the latest PHP error in $php_errormsg.
// This allows for more elegant display of the error messages without having to
// code a php error handler, which would be overkill for such a simple script.
ini_set('track_errors',TRUE);

mysql_connect('localhost', 'user', 'password');
mysql_select_db('database_name');

// Setup variables to be used later
$geocode_url = "http://local.yahooapis.com/MapsService/V1/geocode";
// Enter your custom appid here
$appid ='sample_appid';

// Get the addresses that have not been geocoded
$address_result = mysql_query("SELECT id, street, city, state, zip FROM addresses WHERE lat IS NULL OR lon IS NULL");

// If the MySQL query returned any rows, loop through them
if(mysql_num_rows($address_result) > 0){
    while ($row = mysql_fetch_assoc($address_result)){
        // appid is required for each call to the yahoo geocode API
        $row['appid'] = $appid;
        // output can be either xml or php
        $row['output'] = 'php';

        // Store the row id for updating the database and remove from $row array
        $id = $row['id'];
        unset($row['id']);

        // Build the query string for sending the request to yahoo
        $query_string = http_build_query($row, '', '&');

        // Get latitute / longitude
        $response = @file_get_contents($geocode_url.'?'.$query_string);

        if($response == false){
            // If the call to the yahoo api failed, display an error message
            echo 'ERROR: '.$php_errormsg;
        }
        else{
            // Get the results of the api call and update the database records
            $response_array = unserialize($response);
            $lat = mysql_real_escape_string($response_array['ResultSet']['Result']['Latitude']);
            $lon = mysql_real_escape_string($response_array['ResultSet']['Result']['Longitude']);
            $update_result = mysql_query("UPDATE addresses SET lat = '$lat', lon = $lon WHERE id = $id");

            // For each record, let the user know if the update was successful. If not, display the mysql error message generated.
            if($update_result == TRUE){
                echo "Added lattitude and longitude for {$row['street']} {$row['city']}, {$row['state']} {$row['zip']}.";
            }
            else{
                echo "Unable to add lattitude and longitude for {$row['street']} {$row['city']}, {$row['state']} {$row['zip']}
                    MySQL Error: ".mysql_error();
            }
        }
        echo '<br />';
    }
}
else{
    echo "Nothing to do.";
}
?>
```
The code is documented pretty well so I'll just point out a couple
things

-   The Yahoo Geocoding API can return either XML or PHP serialized
    objects. I chose PHP serialized objects because it's easier to deal
    with for what I'm trying to do.
-   http\_build\_query does not exist in PHP4. To use this code with
    PHP4 you could use the [PHP\_COMPAT Pear
    Package](http://pear.php.net/package/PHP_Compat).

