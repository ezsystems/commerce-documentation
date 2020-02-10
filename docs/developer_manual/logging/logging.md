# Logging 

## Introduction

In order to be able to reproduce the cause of potential errors, eZ Commerce uses [Monolog](https://github.com/Seldaek/monolog) to keep track of the most important information. The standard implementation writes standard shop-related information into a single log file. Mails and ERP communication are written into the database, in order to make it easier to process them for administrative (HTML-)presentation.

The purpose of this part of the documentation is to describe what has been implemented and to show which steps are necessary to create customized logging.

## GDPR

Some logs can conatin personal informations which can be affected by the GDPR regulations.

By default eZ Commerce does not log the user-id for logging search query. 

``` 
siso_core.default.gdpr.store_user_id_in_logs: false
```

## Important terms used in this document

Monolog

Monolog is a PHP library for logging. It provides classes and interfaces in order to implement basic and more sophisticated logging set-ups. It is also very easy to extend the base implementations of Monolog with its concept of Loggers, Handlers, Processors and Formatters. The basic documentation of these features is not in the scope of this document. Please refer to the [Monolog documentation for an introduction](https://github.com/Seldaek/monolog/tree/master/doc).

Log record

A (Mono)log record is a structured set of data that represents a single log entry. For Monolog, it is an array that contains the elements: *message*, *level*, *datetime*, *channel*, *context* and *extra*.

Log type

A log type or log entity describes log records, which have certain data elements in common. For example: all logged ERP messages include in *context* (among other): *measuring\_point*, *message\_identifier*.

Doctrine

Doctrine is a PHP package which [abstracts database access](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/) and provides [object relational mapping](http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/) (i.e. connecting class-objects to database-results). More information can be found at [the official documentation](http://www.doctrine-project.org/projects.html) and the [respective Symfony book](http://symfony.com/doc/current/book/doctrine.html) and [cookbook](http://symfony.com/doc/current/cookbook/doctrine/index.html).

ERP

 **ERP** (Enterprise resource planning) is business [management](http://en.wikipedia.org/wiki/Management "Management") software—typically a suite of integrated applications—that a company can use to collect, store, manage and interpret data from many business activities, including:

  - [Product planning](http://en.wikipedia.org/wiki/Product_planning "Product planning"), cost
  - [Manufacturing](http://en.wikipedia.org/wiki/Manufacturing "Manufacturing") or service delivery
  - [Marketing](http://en.wikipedia.org/wiki/Marketing "Marketing") and sales
  - [Inventory management](http://en.wikipedia.org/wiki/Inventory_management "Inventory management")
  - Shipping and payment

*Source*: Wikipedia

Known ERP software packages:

  - <span class="internal">Microsoft Dynamics NAV (former navision)
  - <span class="internal">Microsoft Dynamics AX  
    
  - SAP

## Related documents

Please keep in mind that Logging is really connected with a lot of different modules in our shop. Be sure to check these out:

  - [ERP communication](ERP-communication_23560973.html) 

  -   - [ERP Logging](ERP-Logging_23560301.html)

  - [MailHelperService](MailHelperService_23560658.html)

  -   - [Mail Logging](Mail-Logging_23560245.html)

## FAQ

### Frequently Asked Questions

Which log entries are stored in the DB?

By default ERP messages and emails are stored in the DB.

All other log entries are stored in the .log file.

I'm getting an error, that some \*log\* database tables couldn't be found.

Please update your database schema

**Command line example**

``` 
php bin/console doctrine:schema:update [--dump-sql|--force]
```

I need to log additionally missing translations (or something else) to the DB. What should I do?

If you need to extend the logging with a new DB log record, please read the [cookbook](#).

How do I invoke a simple log instruction

The ID for the standard logging is 'silver\_common.logger'. This is an instance of Monolog\\Logger. Thus, writing a simple debug log in a container action is like:

``` 
$logger = $this->get('silver_common.logger');
$additionalContext = array('exception' => $ex);
$logger->debug('My log message', $additionalContext);
```

I want to log a new created ERP message. How do I do that?

All transmitted messages are logged automatically, because the mandatory transport layer implicitly logs all messages. But if a new transport is implemented (other than the WebConnectorMessageTransport), this implementation hast to take care of logging measuring point a lower level. Please read the chapter [Measuring Points in the ERP logging documentation](https://doc.silver-eshop.de/display/EZC14/ERP+Logging#ERPLogging-Loggingarchitecture:measuringpoints).

How do I log an email that is sent by the shop?

All emails, which are sent using the [MailHelperService](https://doc.silver-eshop.de/display/EZC14/MailHelperService), are logged implicitly.

How can I avoid, that the password will be logged in the DB?

If the email contains a password, that should not be logged in the DB, you have to specify this password as a template parameter.

[MailHelperService](https://doc.silver-eshop.de/display/EZC14/MailHelperService) will replace the template parameter *'password'* with *'\*\*\*'.*

Where can I find the log file?

 The standard log file is (relative to the shop's root directory) located at `var/logs/silver.eshop.log`

More precisely, Monolog's [StreamHandler](https://github.com/Seldaek/monolog/blob/master/doc/02-handlers-formatters-processors.md#log-to-files-and-syslog) is responsible for writing logs into files. The path the destined file is a [constructor parameter](https://github.com/Seldaek/monolog/blob/master/src/Monolog/Handler/StreamHandler.php#L33) of the StreamHandler. For an example service configuration, please have a look at the [ERP Logging](https://doc.silver-eshop.de/display/EZC14/ERP+Logging#ERPLogging-Configuration).

## Templating

There are no templates for logging.

## Cookbook

[Logging Cookbook For Database Logging](#)

## API

[Logging - API](Logging---API_23560233.html)
