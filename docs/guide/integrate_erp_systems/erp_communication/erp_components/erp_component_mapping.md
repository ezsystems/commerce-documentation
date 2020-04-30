# ERP Component: Mapping

Mapping is used to transform a specific data format into another. There is an abstract class AbstractMessageMappingService which defines the interface for all Mapping services:

``` php
/**
 * Definition of the mapping API.
 */
abstract class AbstractDocumentMappingService
{
    /**
     * This method is destined to map requests.
     *
     * Since is not possible to predict
     * what type of data might be available, no data type for $documentData and
     * return is defined. But it is recommended to use string or array and
     * having the returning result of the same type as the passed data.
     *
     * @param mixed $documentData
     * @param mixed $messageCode
     * @return mixed
     */
    abstract public function mapRequest($documentData, $messageCode);

    /**
     * This method is destined to map responses.
     *
     * Since is not possible to predict
     * what type of data might be available, no data type for $documentData and
     * return is defined. But it is recommended to use string or array and
     * having the returning result of the same type as the passed data.
     *
     * @param mixed $documentData
     * @param mixed $messageCode
     * @return mixed
     */
    abstract public function mapResponse($documentData, $messageCode);
}
```

## XsltDocumentMappingService

This is a mapping service, that uses an XSLT processor to perform the mapping.

Currently the XSL files have to be stored into two special directories inside of the `/app/Resources` directory:

- xslbase: This directory should contain the standard implementation.
- xsl: This directory should contain specific adaptations of selected message mappings.

The files in these directories follow a specific naming scheme: `request|response.<messageCode>.xsl` The first part determines the direction of the communication. The second part (`messageCode`) is passed to the `map*()` methods and defines, which massage is being mapped. A `FileLocatorInterface` is used to resolve the XSL files.

Example request.productsearch.xsl:

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:template match="/CatalogueSearchRequest">
        <PRODUCT_SEARCH>
            <CUSTOMER_NUMBER><xsl:value-of select="PartyIdentification/ID" /></CUSTOMER_NUMBER>
            <!-- CatalogueItemIdentification|StandardItemIdentification maybe? -->
            <ARTICLE_AID><xsl:value-of select="CatalogueLine/Item/ManufacturersItemIdentification/ID" /></ARTICLE_AID>
            <!-- SellersItemIdentification maybe?-->
            <CUSTOMER_AID><xsl:value-of select="CatalogueLine/Item/BuyersItemIdentification/ID" /></CUSTOMER_AID>
            <PRODUCT_TYPE><xsl:value-of select="CatalogueLine/Item/CommodityClassification/ItemClassificationCode" /></PRODUCT_TYPE>
            <DESCRIPTION><xsl:value-of select="CatalogueLine/Item/Description" /></DESCRIPTION>
            <QUANTITY><xsl:value-of select="CatalogueLine/MaximumOrderQuantity" /></QUANTITY>
            <LANGUAGE><xsl:value-of select="Language/LocaleCode" /></LANGUAGE>
        </PRODUCT_SEARCH>
    </xsl:template>
</xsl:stylesheet>
```

Example configuration parameters.yml:

``` yaml
silver_erp.config.messages:
    search_product_info:
        message_class: "Name\\ProjectBundle\\Entity\\Messages\\SearchProductInfoMessage"
        response_document_class: "\\oasis\\names\\specification\\ubl\\schema\\xsd\\OrderResponse_2\\OrderResponse"
        webservice_operation: "http://webserviceurl"
        mapping_identifier: productsearch
```

## Multiple Values / Unbounded Cardinality

There is an important point regarding elements that may occur more than once within a parent element. Since currently, in PHP code, the messages are converted to arrays without considering XSD, it is necessary to mark these elements to be converted to an array, even if it occurs only once. This is done in a special attribute in the target structure. The attribute's name is singleElementArrays and it's content must be the respective elements' unqualified, space-separated names. Example:

- `<Party singleElementArrays="PartyIdentification PartyName">`
- `<PostalAddress singleElementArrays="AddressLine">`

Update:

The current transport implementations (WebConnectorMessageTransport and CurlMessageTransport) do not use arrays for the binding of the message data to the PHP objects anymore. Thus, these attributes are not necessary. But if there are transports in the future, which needs array for their process, this issue has to be considered.

## DefaultValues for undefined attributes

The DefaultValuesService can be used to set default values in outgoing or incoming ERP messages.

Default values will be written while runtime into the objects, if a specified field does exist and is empty. It is possible to set any attribute or the value of a field. If a specified field value/path does not exist, the service will try to build the object hierachy by reflection with the help of the phpDocHeaders from the UBL-classes.

To set a default value, the **complete path** for the specified field must be used. For example:

`BuyerCustomerParty/PostalAddress/Country/IdentificationCode/value: "DE"`

## Configuration via default_values.yml

Configuration format:

``` yaml
parameters:
    silver_erp.config.default_values.request_mappings:
        <Message-Object-Type>:
            <Message-Object-Field>: <Defaultvalue>

    silver_erp.config.default_values.response_mappings:
        <Message-Object-Type>:
            <Message-Object-Field>: <Defaultvalue>
```

Example: Setting setting the value of the field "DocumentCurrencyCode" to "EUR" in an incoming OrderResponse:

``` yaml
parameters:
    silver_erp.config.default_values.response_mappings:
        OrderResponse:
            DocumentCurrencyCode/value: "EUR"
            BuyerCustomerParty/PostalAddress/Country/IdentificationCode/value: "DE"
            Delivery[]/DeliveryAddress/Country/value: "DE"
```

### NoopDocumentMappingService

This is a mapping service, that does no mapping at all. It can be used, if a dependency to an AbstractDocumentMappingService is required but, however, no mapping is necessary nor available.
