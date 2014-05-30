PHP-dbtools
===========

A set of tools to help PHP scripts interact with MySQL databases. 

I built this toolset to simplify my code, avoid unnecessary repetition, help me debug, and improve performance on my apps. It also includes a simple caching system for storing query results. 

Unlike PHP's built-in MySQL interface, this toolset will cache queries in the local filesystem, and won't connect to your database server at all until it encounters a query it can't resolve with the cache. Filesystem caching is the default; you can also configure it to use memcached.

Connecting
----------

```php
  require_once('incl-dbconnect.php');
  $db = new dbLink();
```

Remember, this won't actually open a new connection to your MySQL server. We do that only when you try to execute a query that can't be resolved using the local cache.

Querying
--------

Run a simple query, return an array:
```php
  $my_array = $db->getArray("SQL CODE");
```

Or return a single value:
```php
  $my_value = $db->getOne("SQL CODE");
```

Or execute a query where you don't care about the results (an UPDATE, INSERT, etc.):

```php
  $db->run("SQL CODE");
```

Then get the primary key value of the record you just inserted:
```php
  $primary_key_value = $db->lastInsertId();
```

Caching
-------

You can also tell it to cache the results of your queries to spare your database server. Simply specify how long you want the cache to hang around (in seconds). So to create a 24-hour cache:
```php
  $db->cache(60 * 60 * 24);
```
If you don't want to cache results to the filesystem, use memcached instead:
```php
  $db->memcache(60 * 60 * 24);
  $db->cacheServer("HOST", "PORT");
```

What if you want to cache most of your queries, but one of them isn't suitable for caching? No problem. Just turn caching off:
```php
  $db->cache(60*60*24);
  // Do all the stuff that can be cached
  $db->noCache();
  $uncached_results = $db->getArray("SQL CODE");
  $db->cache(60*60*24);
```

You can also find out whether a query used cached results:
```php
  $results = $db->getArray("SOME SQL CODE");
  if ( $db->cacheUsed() ) {
    print "Query results were cached.";
  } else {
    print "I could not use the cache."; 
  }
```

Debugging
---------

Want to get more verbose error messages so that you can debug your code? Do this:
```php
  $db->debug()
```

You can also get information about the MySQL execution plan for your queries. This is especially useful if your script has a query that's taking an unreasonably long time to execute:

```php
  $db->optimize();
```


