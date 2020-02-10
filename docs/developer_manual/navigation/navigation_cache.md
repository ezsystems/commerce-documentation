#  Navigation - Cache 

## HTTP caching

Current Navigation Module uses HTTP cache to store navigation generation and Stash cache to store URL generation.

After an element is modified in backend, it is added to the queue to be deleted from HTTP cache and stash cache.

Please check this documentation to check how this cache flow works:

[Content Cache Refresh](Content-Cache-Refresh_23560351.html)

## Caching in debug mode

Since on some environments (e.g. DEV) the HTTP caching is not active, the navigation is stored in the stash cache in order to improve the performance.

In order to check, if the debug mode is active, following parameter is used:

``` 
$isDebugEnabled = (bool) $this->container->getParameter('kernel.debug')
```

 In other environments (e.g. PROD) the stash cache is not used.

Caching TTL can be configured:

``` 
parameters:    
    siso_core.default.nav_debug_env_ttl: 86400
```
