#  Curl Configuration 

## Curl

``` 
parameters:
    silver_erp.config.curl.target_url: "http://rest.example.host:81/service"
    silver_erp.config.curl.options:
        CURLOPT_HTTPHEADER:
            - "Content-Type: application/xml"
```

### silver\_erp.config.curl.target\_url

This is the URL, which is addressed by cURL to send the message data.

### silver\_erp.config.curl.options

This is an associative array, which lets you define all possible values, that can be set by [curl\_setopt()](http://www.php.net/manual/en/function.curl-setopt.php). This parameter defines the default values, which are used for every message. Using the override parameter in the respective message configuration, this can be overridden. See section **silver\_erp.config.messages**.

### silver\_erp.config.messages

There are some transport specific settings in this parameter:

  - Firstly, it is possible to define a different URL which should be used for the communication. That URL is configured in the 'webservice\_operation' field.
  - Secondly, you can override the values in the silver\_erp.config.curl.options parameter. This is done in an additional field 'override' (see example).

Example:

``` 
    silver_erp.config.curl.options:
        CURLOPT_HTTPHEADER:
            - "Content-Type: application/xml"

    silver_erp.config.messages:
        search_product_info:
            message_class: "Namespace\\ExampleMessage"
            response_document_class: "\\oasis\\names\\specification\\ubl\\schema\\xsd\\OrderResponse_2\\OrderResponse"
            webservice_operation: "http://otherhost.example.com/example-message"
            mapping_identifier: "example"
            override:
                silver_erp.config.curl.options:
                    CURLOPT_HTTPPROXYTUNNEL: true
                    CURLOPT_PROXY: "proxy.host:1234" 
```

In this case, for the search\_product\_info message, additionally to the CURLOPT\_HTTPHEADER some proxy settings are set up in the cURL handle: CURLOPT\_HTTPPROXYTUNNEL and CURLOPT\_PROXY.

## Messages

Messages are configured in, currently, a single parameter.

### silver\_erp.config.messages

This parameter is an associative array, which itself holds further associative arrays. The top-level keys are identifiers for the messages which are currently not evaluated in the process logic. It is only useful to document the names and distinguish the messages. The keys with in particular message configuration are the following:

  - message\_class: This is the fully qualified classname of the class, that holds the request and response document instances. It is used by the transport implementations to determine the configuration for the given message object.
  - response\_document\_class: This is the class, which is shall be instantiated by the transport implementation with the received data of the response.
  - webservice\_operation: This is a string value, which determines the triggered operation of the transport. Its specific value depends on the transport implementation. Please look at the particular transport documentation (above).
  - mapping\_identifier: This is a string, which configures the mapping identifier for this specific message. Please look at the mapping documentation for more information about this.

Example:

``` 
parameters:
    silver_erp.config.messages:
        calculate_sales_price:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Message\\CalculateSalesPriceMessage"
            response_document_class: "oasis\\names\\specification\\ubl\\schema\\xsd\\OrderResponse_2\\OrderResponse"
            webservice_operation: SV_OPENTRANS_CALCULATE_PRICE
            mapping_identifier: calcprice
```
