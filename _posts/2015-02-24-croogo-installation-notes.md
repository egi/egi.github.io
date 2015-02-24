---
layout: post
title: "Croogo Installation Notes"
description: 
headline: 
modified: 2015-02-24
category: personal
tags: []
image: 
  feature: 
comments: true
mathjax: 
---

It should be fairly easy to install Croogo, because I already familiar with CakePHP. 
Somehow, I was facing difficulties due to forgetting that my Apache and PHP setup was new in my OSX Yosemite.


# Installation Procedure

1. Use composer

   ~~~ bash
   $ composer create-project croogo/app appname
   $ cd appname
   $ composer install
   ~~~

2. prepare database

   ~~~ bash
   $ mysqladmin create-database appname
   ~~~

3. use command line installer

   ~~~ bash
   $ cd appname
   $ Console/cake install.install
   ~~~

4. done.

# Q&A

- **/appname/install not found.**

  Check for mod_rewrite setting on apache. Most likely it was not set yet.

- **On admin page, i did not see contents on column action or status.**

  It should contains icons. You need to rebuild using make

  ~~~ bash
  $ Console/cake croogo make
  ~~~

- **table types not found**

  You might following manual installation that are not explaining how to build the initial database setup. 
   You need to install this using the command line installer or web installer. 
   It will build the initial databases for you.

   Other issue might be related to cache files that are outdated. 
   Deleting cache files might solve the problem.

- **url rewriting not working**

  Are you using apache 2.4? 
   You need to use `Require all granted` command instead of `Order/Deny/Allow` command to AllowOverride via htaccess file.
   You also need to make sure to define allowoverride from the root dir of your apps, not your app/webroot/ dir.

- **cannot connect to MySQL?**

  Try using 127.0.0.1 instead of localhost to define your MySQL host value.
   Other way is to use `unix_socket` field. But the installation script does not support this.

# Reference

- https://github.com/croogo/croogo#installation-using-composer
- http://stackoverflow.com/questions/20752971/cakephp-2-4-3-url-rewriting-module-not-working
- http://stackoverflow.com/questions/9574905/cant-get-going-with-mysql-in-cakephp
- http://dabase.com/blog/AH01630:_client_denied_by_server_configuration/
- http://httpd.apache.org/docs/2.4/upgrading.html#run-time
- http://stackoverflow.com/questions/18392741/apache2-ah01630-client-denied-by-server-configuration
