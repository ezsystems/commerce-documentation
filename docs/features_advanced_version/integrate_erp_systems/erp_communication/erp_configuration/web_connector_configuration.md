# Web-Connector Configuration

In order to use  a Web-Connector a configuration has to be setup.

## General parameters

``` yaml
parameters:
    siso_erp.default.web_connector.service_location: "http://mydomain.com/mywebconnector"
    siso_erp.default.web_connector.username: admin
    siso_erp.default.web_connector.password: passwo
    siso_erp.default.web_connector.soapTimeout: 5
    siso_erp.default.web_connector.erpTimeout: 5

    siso_erp.default.web_connector.allow_self_signed_ssl: true
```

!!! note

    Please check the configuration for the Web-Connector URL in the Backend (Configuration settings) as well! The backend settings will set the default setting and might win.

### siso_erp.default.web_connector.url

Currently, it's only possible to work in non-WSDL mode for the Web-Connector communication. The service location defines the remote service URL of the Web-Connector.

### siso_erp.default.web_connector.username

Defines the user name for Web-Connector

### siso_erp.default.web_connector.password

Defines the password for Web-Connector

### siso_erp.default.web_connector.soapTimeout

Defines the soap timeout.

### siso_erp.default.web_connector.erpTimeout

Defines the timeout for ERP.

### siso_erp.config.messages

There is a transport specific setting in this parameter:

- The 'webservice_operation' parameter is used to define the Web-Connector's SOAP operation, which is called by the transport.

Example:

``` yaml
parameters:
    silver_erp.config.messages:
        calculate_sales_price:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Message\\CalculateSalesPriceMessage"
            response_document_class: "oasis\\names\\specification\\ubl\\schema\\xsd\\OrderResponse_2\\OrderResponse"
            webservice_operation: SV_OPENTRANS_CALCULATE_PRICE
            mapping_identifier: calcorder
            debug: true # true|omit this field completely
```

### siso_erp.default.web_connector.allow_self_signed_ssl

Defines for SOAP request if self signed SSL certificate is allowed.

## Check ERP status

eZ Commerce Advanced provides a feature which should avoid that the ERP system will be contacted in case of an error. 

When an ERP system is not available it might lead to long response times since the ERP might not respond with an error immediately and a time out may occur.

If a message to the ERP system fails the shop will send a test message to the ERP in order to check

- if it is a general issue and the ERP is offline or
- if just this one message failed

If it is a general error the shop will set the ERP connection to offline. All requests send after setting the connection to offline will not be send to the ERP and will immediately handled as an error. 

This setting will set the ERP 60 seconds offline before another request will be sent. 

``` 
siso_erp.erp_semaphore.max_lock_time: 60
```

The status will be stored in stash using the key defined in siso_erp.erp_semaphore.stash_item_id

## Configuration for Web-Connector installation

The Web-Connectors supports the mapping of simple XML messages from one (source) structure to another (target) structure. The rules for that mapping are defined in XSL files in a configurable directory (*mapping/nav_nas* is currently default). This configured directory must contain two subdirectories: *xslbase* and *xsl*. The base mapping rules are delivered in the *xslbase* directory. Variations from the base mapping can be overriden in the *xsl* directory (files must have the same name). For more information, please have a look at the Web-Connector documentation.

This mapping mechanism is available in the new Symfony based eZ Commerce (Advanced version only), too. This mapping defines the translation of the UBL based message to the format used by the ERP system.  

In order to activate this mapping in the Web-Connector setup the corresponding configuration file (e.g. config/custom.php) has to be extended:

``` 
$cfg->setSetting('Mapping', 'XsltPathOffset', 'mapping/noop/');
```
