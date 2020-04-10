# Logging API

The logging in the eZ Commerce is based on Monolog. There is no separate logging API definition. But for the purpose of persisting logs within a database, the Monolog API implementation has been extended by Doctrine-based classes and an interface.

This is just an API reference. For a more detailed description of how Doctrine-based Monolog logging was implemented and should be used, please have a look at [the logging cookbook](#).

## Important service classes and interfaces of Monolog

### Psr\Log\LoggerInterface

This interface SHOULD **always** be used as the dependency / service for logging. 

In the following you will find the list of the logging methods (*in order of importance, descending*). The last method, log(), SHOULD only be used if the log level of a message can only be determined at runtime. For more information about the PSR logging standard please read [the official specification](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md).

```
public function emergency($message, array $context = array());
public function alert($message, array $context = array());
public function critical($message, array $context = array());
public function error($message, array $context = array());
public function warning($message, array $context = array());
public function notice($message, array $context = array());
public function info($message, array $context = array());
public function debug($message, array $context = array());
public function log($level, $message, array $context = array());
```

### Monolog\Logger

This class implements the LoggerInterface but, for historical reasons, provides its own logging methods (which correspond to the interface, although are named differently).

!!! note

    Although huge parts of the shop rely on `Monolog\Logger` directly, it's RECOMMENDED that all new implementations rely on the PSR LoggerInterface instead of explicitly refer to this class.

Because of the recommendation above, I will omit the documentation of the Monolog specific methods here.

## Base classes of Doctrine-based logging

### Siso\Bundle\ToolsBundle\Entity\LogRepositoryInterface

In order to make database logging possible, for every type of log records (mail log, erp log) a specific Doctrine entity class and its respective repository class must be created.

The LogRepositoryInterface must be implemented by every repository class in order to work with the DoctrineHandler.

#### public function createNewLog(array $logRecord, $persist = false);

Parameters:

```
@param array $logRecord The record as created by Doctrine loggers.
@param bool $persist If true, call save() implicitly.
@return Siso\Bundle\ToolsBundle\Entity\AbstractLog
```

Instantiate a new log record entity.
Expected keys for `$logRecord`:

- channel: string (The current monolog channel)
- level: int (numeric log level)
- datetime: \DataTime (date-time of the log message)
- message: string (message to log)
- context: array (monolog context)
- extra: array (additional data from monolog processors)
 
#### public function save($logEntity);

Parameters:

`@param AbstractLog $logEntity A log entity, created by createNewLog()`

Persists the given log entity.

### Siso\Bundle\ToolsBundle\Entity\AbstractOrmLogRepository

This implementation of LogRepositoryInterface uses doctrines ORM API by deriving from EntityRepository.

The class is abstract, but implements the LogRepositoryInterface completely. It is intended to be extended for concrete entity implementations of AbstractLog and, therefore, MUST NOT instantiated directly.

It's RECOMMENDED to extend this class for any ORM based logging entities.

### Siso\Bundle\ToolsBundle\Entity\AbstractLog

Abstract class for all Log entities.

It is not persistable by itself and MUST be derived by e.g. a fully mapped doctrine entity class.

The attributes in the class MUST be considered in any DBMS mapping, as well.

!! note

    For any deriving class that uses Symfony's annotation based configuration for object relational mapping, the base attributes must be overridden and enriched with the respective ORM tags in their DocBlocks.

```
/**
 * @var integer
 */
protected $id;
/**
 * @var \DateTime
 */
protected $logTimestamp;
/**
 * @var string $logChannel
 */
protected $logChannel;
/**
 * @var integer
 */
protected $logLevel;
/**
 * @var string
 */
protected $logMessage;
```

## Monolog extension

The classes that extend Monolog by Doctrine-based persistence do not provide a public API. Instead, they implement the public API of Doctrine. I will only list the created classes here together with their descriptions and how they have to be used in the service container.

### Siso\Bundle\ToolsBundle\Service\Logging\DoctrineFormatter

Implements `Monolog\Formatter\FormatterInterface`.

This is a Monolog formatter class for the DoctrineHandler. It serializes all non-scalar values, so that they can be stored in to database fields.

**Service definition**

``` xml
<!-- Arguments are define here with their default values. They could be omitted -->
<service id="siso_module.logging_formatter.doctrine" class="Siso\Bundle\ToolsBundle\Service\Logging\DoctrineFormatter">
    <argument type="string">8</argument> <!-- Nesting level for recursion -->
    <argument type="string">php</argument> <!-- Serialize format (php|json) -->
    <argument type="string">true</argument> <!-- Convert exception traces to string instead of array -->
</service>
```

### Siso\Bundle\ToolsBundle\Service\Logging\DoctrineHandler

Extends Monolog\Handler\AbstractProcessingHandler

Monolog handler class, that writes log records into Doctrine entities.

**Service definition**

``` xml
<!-- A doctrine handler must be assigned to a specific entity class and it's repository -->
<service id="siso_module.log_type.logging_handler.doctrine" class="Siso\Bundle\ToolsBundle\Service\Logging\DoctrineHandler">
    <argument type="service" id="siso_module.log_type.logging_repository.doctrine" /> <!-- The service ID of the repository class -->
    <call method="setFormatter">
        <argument type="service" id="siso_module.logging_formatter.doctrine"/> <!-- The service id of the previously defined DoctrineFormatter -->
    </call>
</service>
```

**Handler injection**

``` xml
<service id="siso_module.log_type.logger" class="%monolog.logger.class%">
    <argument>log_channel</argument>
    <!-- Handlers are injected with a method call -->
    <call method="pushHandler">
        <argument type="service" id="siso_module.log_type.logging_handler.doctrine"/>
    </call>
</service>
```

### Siso\Bundle\ToolsBundle\Service\Logging\RequestDataProcessor

This monolog data processor adds the request-, session- and user-id to log messages.

**Service definition**

``` xml
<service id="siso_tools.logging_processor.request_data" class="%siso_tools.logging_processor.request_data.class%">
    <argument type="service" id="session" />
    <argument type="service" id="ezpublish.api.repository" />
</service>
```

**Processor injection**

``` xml
<service id="siso_module.log_type.logger" class="%monolog.logger.class%">
    <argument>log_channel</argument>
    <call method="pushHandler">
        <argument type="service" id="siso_module.log_type.logging_handler.doctrine"/>
    </call>
    <!-- Processors are injected with a method call -->
    <call method="pushProcessor">
        <argument type="collection"> <!-- Processors must be injected as a callback (array(Object, 'methodName')) -->
            <argument type="service" id="siso_tools.logging_processor.request_data" />
            <argument type="string">processRecord</argument>
        </argument>
    </call>
</service>
```
