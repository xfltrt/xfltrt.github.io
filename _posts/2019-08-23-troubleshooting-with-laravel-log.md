---
layout: post
published: true
category: development
title: Using Laravel.log for Basic Troubleshooting
---
We received a complain from an external user on our in-house Laravel application. She was having trouble uploading some files onto our application. This was my first Laravel project, so I didn't have tools like 
[Laravel Telescope](https://github.com/laravel/telescope) to help pin point the error.

My first instinct was to replicate the error. I managed to do this by uploading any files other than pdf. The controller only allows jpg, png, and pdf or certain size limit. There's no issue if the files are beyond this scope as there was exception handling.

Okay, I found what causes it, but I'll need to enable debug mode, or so I thought. I know I'm suppose to fix this on the development environment but I didn't have it with me. Enabling debug mode on a production server is definitely a no.

Fortunately, the default log is working well. I only need to look at it to find the exact error. I came up with the following command to filter only Laravel error within the month and year.

``` bash
cat storage/logs/laravel.log | grep laravel.ERROR | grep "2019" | less
```

It was horror. I realised the bug has been affecting more than 20 users just this quarter. The basic troubleshooting workflow follow suit. It was easy to fix the error from there on.

I learn that it is important to review the log, daily when possible. And I'm sure to have tools and notifications installed on my next project.
