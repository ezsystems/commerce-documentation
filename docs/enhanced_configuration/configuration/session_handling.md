# Session handling

The default session handling should be changed, because after clearing the cache all users will be logged out.  
Session now are handled with PDO. There is a possibility to switch also to memcache. 

## Session table

You need to create a session table in the DB if DB storage is used

``` sql
CREATE TABLE session
(
    session_id varchar (255) PRIMARY KEY NOT NULL,
    session_value longtext NOT NULL,
    session_time INT NOT NULL
);
```

## Configuration

You need to change the configuration:

### Setup name for siteaccesses

It is very important to set a session name. Otherwise eZ platform will generate a unique name per siteaccess which causes issues if you switch e.g. the language/siteacess and users cannot share a basket and login across siteaccesses.

``` yaml
site_group:
    session:
        name: eZCommerce
        cookie_lifetime: 86400
```

### General settings

`config.yml`:

``` yaml
framework:
    ...
    session:
        #you can switch between pdo or memcache
        handler_id: session.handler.pdo

services:
    pdo:
        class: PDO
        arguments:
            - "mysql:host=%database_host%;dbname=%database_name%"
            - "%database_user%"
            - "%database_password%"
        calls:
            - [setAttribute, [3, 2]] # \PDO::ATTR_ERRMODE, \PDO::ERRMODE_EXCEPTION

    session.handler.pdo:
        class:     Symfony\Component\HttpFoundation\Session\Storage\Handler\PdoSessionHandler
        arguments: ["@pdo", "%pdo.db_options%"]

    session.memcache:
        class: Memcache
        calls:
            - [addServer , [%session_memcache_host%, %session_memcache_port%]]
    session.handler.mc:
        class: Symfony\Component\HttpFoundation\Session\Storage\Handler\MemcacheSessionHandler
        arguments: ["@session.memcache"]

parameters:    
    pdo.db_options:
        db_table:    session
        db_id_col:   session_id
        db_data_col: session_value
        db_time_col: session_time
    session_memcache_host: 127.0.0.1
    session_memcache_port: 11211
```
