#  Caching - FAQ 

How to solve the problem that the basket preview is not up to date and the box showing the user logged in as well?

This might be caused by a invalid configuration.

Please check if the setting in ezplatform.yml for purging the cache is set correctly:

``` 
ezpublish:
    http_cache:
        purge_type: local
```

  - local means the proxy of Symfony is used
  - if varnish or a CDN is used set purge\_type to http
