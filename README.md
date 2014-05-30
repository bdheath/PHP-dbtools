PHP-dbtools
===========

A set of tools to help PHP scripts interact with MySQL databases. 

I built this toolset to simplify my code, avoid unnecessary repetition, help me debug, and improve performance on my apps. It also includes a simple caching system for storing query results. 

Unlike PHP's built-in MySQL interface, this toolset will cache queries in the local filesystem, and won't connect to your database server at all until it encounters a query it can't resolve with the cache. 

How it works:

'''php
  require_once('incl-dbconnect.php');
  $db = new dbLink();
'''
