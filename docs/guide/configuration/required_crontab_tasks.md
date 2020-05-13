# Required crontab tasks

## Recurring tasks in crontab

### Remove translation and navigation caches

The shop collects changes regarding translations (textmodules) and navigation.
If there are changes (e.g. performed in the backend) the cache will be refreshed.   

``` 
# Checks for changes and refresh cache
*/5 * * * * cd '/var/www/my_project' && /usr/bin/php bin/console silversolutions:cache:refresh --env=prod
```

### Send lost orders to the ERP

Lost orders can be resent using a command-line tool. We recommend running this tool e.g. every 5 minutes.

``` 
# resends lost orders every 5 minutes
*/5 * * * * cd '/var/www/my_project' && /usr/bin/php bin/console silversolutions:lostorder:process --env=prod
```

### Use job-queue-system

Please check the documentation for [JMSJobQueueBundle](http://jmsyst.com/bundles/JMSJobQueueBundle/master/installation)

In case the suggested `supervisord` configuration cannot be used, a cronjob can be set up: https://github.com/schmittjoh/JMSJobQueueBundle/issues/205

### Calculate statistical data for active sessions

The dashboard uses statistical data about sessions recorded in a db table. This command line will refresh the data every 5 minutes. 

!!! note

    This feature is available only if sessions are handled in the database.

``` 
*/5 * * * * cd /var/www/my_project &&  /usr/bin/php bin/console silversolutions:sessions write_stat --env=prod
```
