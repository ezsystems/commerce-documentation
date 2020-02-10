#  Install Order history 

## Order history

Order history is only enabled for registered customers with customer number

## How to configure the order history

1, Clone the order history repository into vendor/silversolutions

``` 
//from project root
cd vendor/silversolutions
git clone http://git.extranet.silversolutions.de/silver.e-shop.orderhistory.git silver.orderhistory 
```

3, Regenerate the autoload namespaces

``` 
//from root
php composer.phar self-update
php composer.phar dump-autoload
```

4, Enable the Order History Bundle in the Kernel

**\~/ezpublish/EzPublishKernel.php**

``` 
 public function registerBundles()
    {
        $bundles = array(
            ...
            new Siso\Bundle\OrderHistoryBundle\SisoOrderHistoryBundle(),
        );
```

6, Import routing entries in the configuration

**\~/ezpublish/config/routing.yml**

``` 
siso_order_history:
    resource: "@SisoOrderHistoryBundle/Resources/config/routing.yml"
    prefix:   /

... 
```

7, Clear the cache and regenerate the assetics

``` 
php bin/console cache:clear
php bin/console assetic:dump
```

8, Make sure that you have configured all messages

``` 
    silver_erp.config.messages:
       ...
        invoice_detail:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\InvoiceDetailMessage"
            response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\Invoice"
            webservice_operation: "SV_OPENTRANS_GET_ORDERSTATUS"
            mapping_identifier: ""
        orderdetail:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\OrderDetailMessage"
            response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\OrderDetailResponse"
            webservice_operation: "SV_OPENTRANS_GET_ORDERSTATUS"
            mapping_identifier: ""
        delivery_note_detail:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\DeliveryNoteDetailMessage"
            response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\DeliveryNote"
            webservice_operation: "SV_OPENTRANS_GET_ORDERSTATUS"
            mapping_identifier: ""
        orderlist:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\OrderListMessage"
            response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\OrderListResponse"
            webservice_operation: "SV_OPENTRANS_GET_ORDERLIST"
            mapping_identifier: ""
        invoice_list:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\InvoiceListMessage"
            response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\InvoiceList"
            webservice_operation: "SV_OPENTRANS_GET_ORDERLIST"
            mapping_identifier: ""
        delivery_note_list:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\DeliveryNoteListMessage"
            response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\DeliveryNoteList"
            webservice_operation: "SV_OPENTRANS_GET_ORDERLIST"
            mapping_identifier: ""
```
