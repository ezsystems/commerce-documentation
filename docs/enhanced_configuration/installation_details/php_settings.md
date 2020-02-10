#  PHP settings 

## Session settings

A parameter in the php.in (e.g. /etc/php5/apache2/php.ini) might have to setup since it reduces the lifetime of a session:

``` 
session.gc_maxlifetime = 86400
```

It can be set in a .htaccess as well:

``` 
php_value session.gc_maxlifetime 86400
```
