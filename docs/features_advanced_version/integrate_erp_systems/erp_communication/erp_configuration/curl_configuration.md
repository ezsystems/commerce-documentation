# Curl Configuration

## Curl

``` yaml
parameters:
    silver_erp.config.curl.target_url: "http://rest.example.host:81/service"
    silver_erp.config.curl.options:
        CURLOPT_HTTPHEADER:
            - "Content-Type: application/xml"
```

### silver_erp.config.curl.target_url

This is the URL, which is addressed by cURL to send the message data.

### silver_erp.config.curl.options

This is an associative array, which lets you define all possible values, that can be set by [curl_setopt()](http://www.php.net/manual/en/function.curl-setopt.php). This parameter defines the default values, which are used for every message. Using the override parameter in the respective message configuration, this can be overridden. See section **silver_erp.config.messages**.

### silver_erp.config.messages

There are some transport specific settings in this parameter:

  - Firstly, it is possible to define a different URL which should be used for the communication. That URL is configured in the 'webservice_operation' field.
  - Secondly, you can override the values in the silver_erp.config.curl.options parameter. This is done in an additional field 'override' (see example).

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

In this case, for the search_product_info message, additionally to the CURLOPT_HTTPHEADER some proxy settings are set up in the cURL handle: CURLOPT_HTTPPROXYTUNNEL and CURLOPT_PROXY.

## Messages

Messages are configured in, currently, a single parameter.

### silver_erp.config.messages

This parameter is an associative array, which itself holds further associative arrays. The top-level keys are identifiers for the messages which are currently not evaluated in the process logic. It is only useful to document the names and distinguish the messages. The keys with in particular message configuration are the following:

- message_class: This is the fully qualified classname of the class, that holds the request and response document instances. It is used by the transport implementations to determine the configuration for the given message object.
- response_document_class: This is the class, which is shall be instantiated by the transport implementation with the received data of the response.
- webservice_operation: This is a string value, which determines the triggered operation of the transport. Its specific value depends on the transport implementation. Please look at the particular transport documentation (above).
- mapping_identifier: This is a string, which configures the mapping identifier for this specific message. Please look at the mapping documentation for more information about this.

Example:

``` yaml
parameters:
    silver_erp.config.messages:
        calculate_sales_price:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Message\\CalculateSalesPriceMessage"
            response_document_class: "oasis\\names\\specification\\ubl\\schema\\xsd\\OrderResponse_2\\OrderResponse"
            webservice_operation: SV_OPENTRANS_CALCULATE_PRICE
            mapping_identifier: calcprice
```
