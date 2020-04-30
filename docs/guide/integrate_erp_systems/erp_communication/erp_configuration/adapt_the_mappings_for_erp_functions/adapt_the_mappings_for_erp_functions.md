# Adapt the mappings for ERP functions

eZ Commerce is using a flexible way to map the reuqest and response between the shop and the ERP system.

By default eZ Commerce comes with a prepared mapping stored in the vendor bundle. The mappings can be extended in your project bundles.

Please note that your bundle has to be registered as a mapping bundle:

``` yaml
siso_erp.default.mapping_bundles:
    - 'MyProjectBundle'
    - 'SilversolutionsEshopBundle'
    
```

## Where to find the mappings?

For each ERP message a mapping can be defined. In the following example the mapping identifier is "createorder"

``` yaml
siso_erp.default.message_settings.createsalesorder:
    message_class: "Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\CreateSalesOrderMessage"
    response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\OrderResponse"
    webservice_operation: "SV_OPENTRANS_CREATE_ORDER"
    mapping_identifier: "createorder"
```

The mapping files are using xslt and are located in vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/mapping/wc3-nav/xsl. The folder 'wc3-nav' can be configured in order to support different mappings in one installation (siso\_erp.default.target\_code: 'wc3-nav').

The responsible mappings files for the createorder message are named:

- request.createorder.xsl
- response.createorder.xsl

If you want to override the mapping please create the mapping files in your Bundle structure:

- MyBundle/Resources/mapping/wc3-nav/

Please check the chapter [Create a standard message (UpdateCustomer)](../../guides/how_to_create_a_new_erp_message/create_a_standard_message_updatecustomer.md) in order to learn how to adapt the mapping using xslt.
