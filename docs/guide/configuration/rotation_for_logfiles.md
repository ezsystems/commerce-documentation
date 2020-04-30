# Rotation for logfiles

For systems using prod as environment:

- `var/logs/prod.log`
- `var/logs/silver.eshop.log`
- `var/logs/prod-siso.eshop.erp.log`

## logrotate

The `logrotate` utility is a standard tool in Linux systems. It is run by `cron.daily` once a day at 6:25 am (on Ubuntu systems). When `logrotate` runs, it read all the configuration scripts in `/etc/logrotate.d/`

This directory contains already several scripts for e.g. apache2, samba, apt, etc.

## Configuration for rotating logs

Create a new configuration file for `logrotate` in the following path:

``` bash
sudo vi /etc/logrotate.d/silver-eshop
```

the content should be this:

``` yaml
/var/www/projects/<your-project>/var/logs/prod.log /var/www/projects/<your-project>/var/logs/silver.eshop.log /var/www/projects/<your-project>/var/logs/prod-siso.eshop.erp.log {
    su www-data www-data
    daily
    size 50M
    rotate 30
    missingok
    create 674 devel devel
    compress
}
```

If the logfiles are getting big very quickly, it's possible to run the `logrotate` once per hour by putting it in `cron.hourly` (it will run at 17 minutes every hour)

``` bash
mv /etc/cron.daily/logrotate /etc/cron.hourly
```
