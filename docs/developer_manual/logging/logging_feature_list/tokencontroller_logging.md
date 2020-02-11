#  TokenController Logging 

## General

Logging was added to the [TokenController](TokenController_23560947.html). If a token is processed it will be logged in a file or database. The default configuration is set for database logging:

**ses\_gdpr\_log**

``` 
mysql> select * from ses_gdpr_log;

+----+---------------------+-------------------+-----------+---------------------------------------------------------------------------------------------------------------------------+--------------------------+---------+

| id | log_timestamp       | log_channel       | log_level | log_message                                                                                                               | log_mail                 | user_id |

+----+---------------------+-------------------+-----------+---------------------------------------------------------------------------------------------------------------------------+--------------------------+---------+

|  3 | 2019-02-12 16:05:08 | silver_eshop_gdpr |       600 | Called token method: Silversolutions\Bundle\EshopBundle\Services\Token\TokenServiceMethodProcessorService::enableEzUser() | dam@silversolutions.de   | 381     |

+----+---------------------+-------------------+-----------+---------------------------------------------------------------------------------------------------------------------------+--------------------------+---------+
```

## Configuration

File and database logging is activated by adding the "pushHandler" method call with the "gdpr\_log\_handler.doctrine" and/or "gdpr\_log\_handler.file" service as an argument:

**src/Silversolutions/Bundle/EshopBundle/Resources/config/ses\_services.xml** Expand source 

``` 
<service id="siso_core.gdpr_log_repository.doctrine" class="%siso_core.gdpr_log_repository.doctrine.class%">
    <factory service="Doctrine\Common\Persistence\ObjectManager"  method="getRepository" />
    <argument type="string">SilversolutionsEshopBundle:GdprLog</argument>
</service>
<service id="siso_core.gdpr_log_handler.doctrine" class="%siso_tools.logging_handler.doctrine.class%">
    <call method="setLogRepository">
        <argument type="service" id="siso_core.gdpr_log_repository.doctrine" />
    </call>
    <call method="setFormatter">
        <argument type="service" id="siso_tools.logging_formatter.doctrine"/>
    </call>
</service>
<service id="siso_core.gdpr_log_handler.file" class="Monolog\Handler\StreamHandler">
    <argument type="string">%kernel.logs_dir%/%kernel.environment%-siso.eshop.gdpr.log</argument>
</service>
<service id="siso_core.gdpr_logger" class="Symfony\Bridge\Monolog\Logger">
    <argument type="string">silver_eshop_gdpr</argument> <!-- The logging channel -->
    <call method="pushHandler">
        <argument type="service" id="siso_core.gdpr_log_handler.doctrine" />
    </call>
    <!-- File logging is deactivated by default -->
    <!--<call method="pushHandler">
        <argument type="service" id="siso_core.gdpr_log_handler.file" />
    </call>-->
</service>
```
