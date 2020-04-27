# How to store sessions in Memcache using stash and separate them from other stash caches

## Requirements

- Store sessions in Memcached, preferably using stash
- Don't mix session stash cache with other stash caches (SPI, translation , navigation)
- Configure failover session cache server

## Implementation

### Step 1. Configure Memcache servers

Add new stash service to stash configuration, `ezpublish_env.yml`:

``` yaml
session:
    drivers: [ Memcache ]
    inMemory: true
    registerDoctrineAdapter: false
    registerSessionHandler: true
    Memcache:
        prefix_key: harmony_
        retry_timeout: 1
        servers:
            -
                server: 127.0.0.1
                port: 11211
            -
                server: 127.0.0.1
                port: 11212
```

!!! caution

    Don't forget parameter `registerSessionHandler: true`.

### Step 2. Configure session handler

`config_env.yml`:

``` yaml
session:
    handler_id: stash.adapter.session.session_cache
```

## Test cases

### Test 1. Sessions are stored in Memcached

1. Configure just one memcached server
1. Connect with telnet, check that session name are stored as keys
1. Expected: site is working

### Test 2. Failover server works

1. Configure 2 memcached servers on different ports
1. Stop first server
1. Connect with telnet to second server, check that session name are stored as keys
1. Expected: site is working
