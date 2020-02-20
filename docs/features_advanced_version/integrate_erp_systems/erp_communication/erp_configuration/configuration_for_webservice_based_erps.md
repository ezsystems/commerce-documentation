# Configuration for Webservice based ERPs

If your ERP systems offers different URLs/Webservices for each feature you can define the endpoint to the Webservice per messages. The parameter "webservice_wsdl" can be set to the URL of the responsible webservice.  

``` 
siso_erp.default.message_settings.createsalesorder:
    message_class: "Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\CreateSalesOrderMessage"
    response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\OrderResponse"
    webservice_operation: "MyWebserviceMethod"
    mapping_identifier: "createorder"
    webservice_wsdl: "https://mywebservice.com/WS/..../CreateOrderServices"
        
```

- MyWebserviceMethod: The method defined in the WSDL which in this case provides features to create the order in the ERP
- webservice\_wsdl: The URL to the webservice itself
