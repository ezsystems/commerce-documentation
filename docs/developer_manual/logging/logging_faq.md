# Logging FAQ

## Which log entries are stored in the DB?

By default ERP messages and emails are stored in the DB.

All other log entries are stored in the .log file.

## I'm getting an error, that some `log` database tables couldn't be found.

Please update your database schema

``` 
php bin/console doctrine:schema:update [--dump-sql|--force]
```

## How do I invoke a simple log instruction

The ID for the standard logging is `silver_common.logger`. This is an instance of `Monolog\Logger`. Thus, writing a simple debug log in a container action is like:

``` 
$logger = $this->get('silver_common.logger');
$additionalContext = array('exception' => $ex);
$logger->debug('My log message', $additionalContext);
```

## I want to log a new created ERP message. How do I do that?

All transmitted messages are logged automatically, because the mandatory transport layer implicitly logs all messages. But if a new transport is implemented (other than the WebConnectorMessageTransport), this implementation hast to take care of logging measuring point a lower level. Please read the chapter [Measuring Points in the ERP logging documentation](ERP-Logging_23560301.html#ERPLogging-Loggingarchitecture:measuringpoints).

## How do I log an email that is sent by the shop?

All emails, which are sent using the [MailHelperService](MailHelperService_23560658.html), are logged implicitly.

## How can I avoid logging the password in the DB?

If the email contains a password, that should not be logged in the DB, you have to specify this password as a template parameter.

[MailHelperService](MailHelperService_23560658.html) will replace the template parameter *'password'* with *'\*\*\*'.*

## Where can I find the log file?

The standard log file is (relative to the shop's root directory) located at `var/logs/silver.eshop.log`

More precisely, Monolog's [StreamHandler](https://github.com/Seldaek/monolog/blob/master/doc/02-handlers-formatters-processors.md#log-to-files-and-syslog) is responsible for writing logs into files. The path the destined file is a [constructor parameter](https://github.com/Seldaek/monolog/blob/master/src/Monolog/Handler/StreamHandler.php#L33) of the StreamHandler. For an example service configuration, please have a look at the [ERP Logging](ERP-Logging_23560301.html#ERPLogging-Configuration).
