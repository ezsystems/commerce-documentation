# Logging

## Introduction

In order to be able to reproduce the cause of potential errors, eZ Commerce uses [Monolog](https://github.com/Seldaek/monolog) to keep track of the most important information. The standard implementation writes standard shop-related information into a single log file. Mails and ERP communication are written into the database, in order to make it easier to process them for administrative (HTML-)presentation.

The purpose of this part of the documentation is to describe what has been implemented and to show which steps are necessary to create customized logging.

## GDPR

Some logs can contain personal information which can be affected by the GDPR regulations.

By default eZ Commerce does not log the user-id for logging search query. 

``` 
siso_core.default.gdpr.store_user_id_in_logs: false
```

## Related documents

Please keep in mind that Logging is really connected with a lot of different modules in our shop. Be sure to check these out:

- [ERP communication](ERP-communication_23560973.html) 
   - [ERP Logging](ERP-Logging_23560301.html)
- [MailHelperService](MailHelperService_23560658.html)
   - [Mail Logging](Mail-Logging_23560245.html)
