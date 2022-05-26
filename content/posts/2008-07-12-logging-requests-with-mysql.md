---
tags:
- MySQL
date: "2008-07-12T00:00:00Z"
description: Trying to setup a simple activity log using MySQL?  Using this techniquie
  it's easy to get a count of actions performed on your site.
keywords: mysql, simple application logging
title: Logging requests with MySQL
url: /mysql/logging-requests-with-mysql.html
---
One of my recent work projects was publishing a widget which others can add to their websites by copying some code and using it on their site.  Similar to how [YouTube](http://youtube.com) makes it easy to embed their videos elsewhere.

Of course we want to know how and where this widget is being used so we can decide if it's worth doing more widgets in the future, so we needed to include tracking. What we want to know is where the widget is being used, and how many times it is being displayed on those URLs.

The tracking table is very simple:

```sql
CREATE TABLE widget_impressions (
    id int(11) NOT NULL auto_increment,
    referral_url varchar(255) NOT NULL,
    counter int(11) NOT NULL,
    date_created date NOT NULL,
    PRIMARY KEY  (referral_url,date_created),
    UNIQUE KEY id (id)
)
```

Technically, the id column is not needed since the primary key is a
combination of the referral\_url and the date\_viewed field, but it can
be handy in some situations.

When the widget is displayed, the following query is run:

```sql
INSERT INTO widget_impressions
     SET
        referral_url = 'http://example.com',
        date_viewed = now(),
        counter = 1
        ON DUPLICATE KEY UPDATE
        counter = counter + 1;
```

If this is the first visit from http://example.com for the day, a new record will be added to the table. If it's not the first record for the day, the counter will be incremented by one. Using the date type for the date_viewed field means only the date is stored, not the time. If a datetime field were used, this method would not work the same way; we'd have separate entries in the table every time someone viewed the widget.

With this table structure it's very easy to get stats. Say you want to know all of the pages where the widget is displayed and how many times it has been displayed on each:

```sql
SELECT referral_url, SUM(counter) as num_views FROM widget_impressions GROUP BY referral_url;
```
Want to know the 10 sites that are displaying your widget the most?

```sql
SELECT referral_url, SUM(counter) as num_views FROM widget_impressions GROUP BY referral_url ORDER BY num_views DESC LIMIT 10;
```
**References:**

- [MySQL - on duplicate key update](http://bradym.net/mysql/on-duplicate-key-update)
- [Overview of Date and Time Types](http://dev.mysql.com/doc/refman/5.0/en/date-and-time-type-overview.html)
- [now()](http://dev.mysql.com/doc/refman/5.0/en/date-and-time-functions.html#function_now)

