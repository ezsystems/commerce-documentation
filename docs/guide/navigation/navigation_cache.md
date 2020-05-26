# Navigation cache

## HTTP caching

Navigation uses HTTP cache to store the generated navigation, and stash cache to store generated URLs.

After an element is modified in the Back Office, it is added to the queue to be deleted from HTTP cache and stash cache.

See [Content cache refresh](../cache/content_cache_refresh/content_cache_refresh.md) for more information.

## Caching in debug mode

On some environments (e.g. dev) the HTTP caching is not active, so navigation is stored in the stash cache to improve the performance.

In order to check if the debug mode is active, use the following parameter:

``` php
$isDebugEnabled = (bool) $this->container->getParameter('kernel.debug')
```

In other environments (e.g. prod) stash cache is not used.

Caching TTL can be configured in the following way:

``` yaml
parameters:    
    siso_core.default.nav_debug_env_ttl: 86400
```
