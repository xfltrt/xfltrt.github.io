---
layout: post
published: true
category: development
title: Case Sensitive WHERE Clause
---
Today I found a bug in my URL shortener app;

It didn't occur to me that the fields in a table aren't automatically case sensitive. This is a problem for my app because the auto-generated URLs uses base62 (A-Za-z0-9). "a1b2" will forward to the same link as "A1B1".

I learned that this is the expected behaviour for Laravel's default database collation. "UTF8_unicode_ci" isn't case sensitive. To change that, I can use "UTF8_bin" for the whole database. But that introduces another peculiarity to my database when it comes to ORDER BY.

So in this case, I prefer to keep the default charset/collation and just use "UTF8_bin" for one specific table.

To fix this behaviour. I've added these two lines to my migration:

``` php
$table->charset = 'utf8';
$table->collation = 'utf8_bin';
```

And now everything works as intended.

Alternatively, if you're using MySQL version 8+, you only need the following line:

``` php
$table->collation = 'utf8mb4_0900_as_cs'
```
