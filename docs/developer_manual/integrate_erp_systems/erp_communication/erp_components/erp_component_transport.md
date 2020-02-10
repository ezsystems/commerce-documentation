#  ERP Component: Transport

The transport component's task is to provide an interface to the physical transportation of the messages. This includes the serialization of the data into a stream, that the physical transportation is capable to process. Since serialized data is the most abstract form of data (generally a string) the mapping invocation is also placed in this component. But mapping itself is documented in [a separate section](23560398.html).

## Abstract message transport class

All transport classes must derive from AbstractMessageTransport. This class provides a public but final sendMessage() method, that defines a process that all transport classes must provide. It calls an abstract protected internalSendMessage() method, which has to do the actual communication and message processing. Furthermore, it checks a semaphore service which indicates whether the remote ERP service is available and sets that semaphore afterwards if an error occurred during communication.

The overall process, this class is intended to ensure, is:

1.  Dispatch a MessageRequestEvent, which enables listeners to change the request before it is sent.

2.  Do the actual message transportation to the remote service:
    
    1.  Check the semaphore service, if communication is not blocked. ErpDisabledException is thrown and caught if it is blocked.
    2.  call internalSendMessage()
    3.  If ErpUnavailableException was thrown by internalSendMessage(), the semaphore is set to blocked.

3.  Dispatch a MessageResponseEvent, which enables listeners to change the response before it is passed back to the standard business logic. MessageExceptionEvent is dispatched instead if an error occurred.
 
### Events

There are two events defined, which are dispatched during the transport:

  - `\Silversolutions\Bundle\EshopBundle\Message\Event\TransportMessageEvents::REQUEST_MESSAGE = 'silver_eshop.request_message'`
      - The `silver_eshop.request_message` event is dispatched when a message is about to be sent by the transport layer (before a potential mapping).  
        The event listener receives an instance of `Silversolutions\Bundle\EshopBundle\Message\Event\MessageRequestEvent`.
  - `\Silversolutions\Bundle\EshopBundle\Message\Event\TransportMessageEvents::RESPONSE_MESSAGE = 'silver_eshop.response_message'`
      - The `silver_eshop.response_message` event is dispatched when a message was received and unserialized by the transport layer (after a potential mapping).  
        The event listener receives an instance of `Silversolutions\Bundle\EshopBundle\Message\Event\MessageResponseEvent`.
  - `\Silversolutions\Bundle\EshopBundle\Message\Event\TransportMessageEvents::RESPONSE_MESSAGE = 'silver_eshop.exception_message'`
      - This `silver_eshop.exception_message` event is dispatched when an exception occurs while sending the message.  
        The event listener receives an instance of `Silversolutions\Bundle\EshopBundle\Message\Event\MessageExceptionEvent`.

### Test the transport channel

AbstractErpTransport has an additional method: `testCommunication()`

This method is intended to invoke an actual request by using the transport and return the status of the response in form of a boolean value (true = success; false = failure)

Parameters:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Type</th>
<th>Definition</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>string</code></pre></td>
<td><pre><code>&amp; $errorText = &#39;&#39;</code></pre></td>
<td>This optional parameter gets a variable per reference to which an error text is written (if any occurs).</td>
</tr>
</tbody>
</table>

## ERP semaphore service

In order to indicate the status of availability of the remote ERP system, a semaphore service was created. The interface for that service is `Silversolutions\Bundle\EshopBundle\Services\ErpSemaphoreServiceInterface.` This interface provides two methods.

### Method: testAvailability()

This method is intended to check the status of the semaphore. If the semaphore locked, `Silversolutions\Bundle\EshopBundle\Exceptions\ErpDisabledException` is thrown, else nothing happens. The semaphore is locked, if setUnavailable() was invoked before and the timeout didn't expire, yet.

### Method: setUnavailable()

This method is intended to set the status of the semaphore to locked and set/reset the timeout for that status. The timeout has to be set the service implementation somehow. Injection is recommended.

### Implementation: StashErpSemaphoreService

Class: `Silversolutions\Bundle\EshopBundle\Services\StashErpSemaphoreService`

This implementation uses the symfony Stash API for persisting the status of the semaphore between requests. It is configured with the following parameters:

  - *siso\_erp.erp\_semaphore.stash\_item\_id*: A string which identifies the semaphore whithin the stash pool.
  - *siso\_erp.erp\_semaphore.max\_lock\_time*: An integer which defines the semaphore's timeout in seconds. This parameters should be used across all implementations.

This service has a dependency to `Stash\Interfaces\PoolInterface` injected into the constructor.

### Implementation: FileErpSemaphoreService

Class: `Silversolutions\Bundle\EshopBundle\Services\FileErpSemaphoreService`

This implementation uses simple file system functions for persisting the status of the semaphore between requests. It is configured with the following parameters:

  - *siso\_erp.erp\_semaphore.lock\_file*: This defines the path to the file which should be used to determine the status of the semaphore.
  - *siso\_erp.erp\_semaphore.max\_lock\_time*: An integer which defines the semaphore's timeout in seconds. This parameters should be used across all implementations.

This implementation might not be safe for cluster configurations.

## Web-Connector transport

This class is the transport implementation, which communicates with the silver.solutions Web-Connector.

Currently only the SOAP RPC mode is implemented.

**Serializing:**

For serialzing symfony's serializer API is used: <https://github.com/symfony/Serializer>. For this a new (un-)serializer was created: DomXmlEncoder. This class leverages a fork of the XSD2PHP library and is capable to transform the document class instances automatically to XML code and backwards. During deserialization, fields that are not defined in the destinated response class, will be ignored and it will be logged using the logging dependency of the transport.

**Mapping:**

Additionally, if an instance of AbstractDocumentMappingService was injected by the method setMappingService(), the normalized array of the request and response are passed to this mapping service. That means, that the subclass of the mapper must be able to handle array data. For example the XsltDocumentMappingService may be used. If no service was injected, no mapping is done at all.

**Injecting configuration:**

All configuration must be injected using the respective setter methods. These method calls are already defined in the service configuration of the EshopBundle. All default values are defined in the webconnector.yml and may or must be overwritten in the project configuration.

### Configuration parameters (webconnector.yml)

Example:

``` 
    silver_erp.config.web_connector.service_location: "http://webconnector.example.com:81/webconnector/webconnector_opentrans.php5"
    silver_erp.config.web_connector.service_uri: "http://www.silversolutions.de"
    silver_erp.config.web_connector.default_parameters:
        user: serviceUser
        password: $secret!
        erp_parameters:
            timeout: 10000
```

#### silver\_erp.config.web\_connector.service\_location

This is a string field. It defines a URL which directs the Web-Connector's web-service.

#### silver\_erp.config.web\_connector.service\_uri

This is a string field. It is only used, if the service is working in non-WSDL mode. But since WSDL is not implemented yet, it always used.

#### silver\_erp.config.web\_connector.default\_parameters

This is an array map field. All ERP operations of the Web-Connector have the same parameter signature. The last parameter is the given message data. The others can be configured in this configuration parameter:

  - *user*: Web-Connector authentication user name.

  - *password*: Web-Connector authentication user password.

  - *erp\_parameters*: another array map defining additional parameters. Please see Web-Connector documentation for more information.

### Message configuration parameters (messages.yml)

Example:

``` 
    silver_erp.config.messages:
        example_message:
            message_class: "Namespace\\ExampleMessage"
            response_document_class: "Namespace\\DocumentClass"
            webservice_operation: SV_OPENTRANS_CALCULATE_PRICE
            mapping_identifier: example
        another_message: ...
```

silver\_erp.config.messages

This is an array map field. It contains several keys which themselves contain records for the respective message configuration. The keys do not have any functional meaning (yet). They're just for the distinction of the messages in the configuration itself.

The keys of the message records explained:

  - *message\_class*: The fully qualified class name of the message (instance of AbstractMessage)

  - *response\_document\_class*: The fully qualified class name of the response document. This is used to instantiate the response in the transport before the raw data is fed into the instance.

  - *webservice\_operation*: The name of the SOAP web-service operation for this message.

  - *mapping\_identifier*: The base name of the mapping, if any is set up.

  - *debug*: When set, the message's request and response data is written to the log file shorty before and after SOAP call.
