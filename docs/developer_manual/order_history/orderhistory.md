#  Orderhistory 

## Introduction

Orderhistory is an **additional** eZ Commerce module that is based on the standard eZ Commerce functionality. The user has the possibilty to see an overview and a details of his orders or other documents from the ERP system. He will see documents based on online purchases as well as order placed e.g. by a telephone call.

On the detail page he can see the buyer, delivery address, ordered items and some more details about the order, like status, delivery and payment selection.

He can also use the [addToBasket](/pages/createpage.action?spaceKey=EZC14&title=Basket++xxx&linkCreation=true&fromPageId=23560603) functionality here and speed up the order process if he e.g. often orders the same items.

Orderhistory supports different document types:

  - order
  - invoice
  - delivery note
  - credit memo

## Before you start 

IMPORTANT

Make sure to the users have been granted <span class="b5" style="color: rgb(0,0,0);">access to the orderhistory. "**siso\_policy orderhistory\_view" must be set\!**

## Installation

see [Install Order history](Install-Order-history_23560562.html)

## Configuration

Orderhistory is using semantic configuration, so it only exposes parameters that are allowed to be configurable.

However it is possible to override this configuration by siteaccess. Therefore an event is thrown, before the configuration is used and you can implement a listener, that will be able to change this configuration. eZ Commerce makes usage of this event for displaying of the [local orders](OrderHistory---Local-Orders-when-ERP-not-available_23560888.html). Check our [FAQ](Orderhistory---FAQ_23560629.html) to find out, how to implement a new configuration listener.

``` 
siso_order_history:
    list:
        max_document_count: 30
    
    document_types:
        - invoice
        - delivery_note
        - credit_memo
        - order
    
    default_document_type: invoice
    
    #valid values: <integer> hours or <integer> minutes and so on, the date will be calculated: <now> - given time
    date:
        start: 2 years
        end: 0 days
        max_start: 4 years
    
    # default sorting for columns in list page
    # valid values:
    # - numeric-comma (for numbers which use a comma as the decimal place like currency),
    # - datetime      (for datetime in the format <dd.mm.YYYY HH:mm> or <dd.mm.YYYY>
    # - false         (disable sorting for a column)
    # for more see: https://www.datatables.net/plug-ins/sorting/
    default_list_sort:
        invoice:
            - numeric-comma
            - datetime
            - numeric-comma
            - numeric-comma
        delivery_note:
            - numeric-comma
            - datetime
        order:
            - numeric-comma
            - datetime
            - numeric-comma
            - numeric-comma
        credit_memo:
            - numeric-comma
            - datetime
            - numeric-comma
            - numeric-comma
    
    # default column sorting for list view
    # valid values: [[<column_index>,<sort_order>], ... , [<column_index>,<sort_order>]]
    # column_index of first column is 0!
    default_list_sort_column:
        #example multiple sorting
        #invoice: [[0, 'desc'], [2, 'asc']]
        invoice: [[1, 'desc']]
        delivery_note: [[1, 'desc']]
        order: [[1, 'desc']]
        credit_memo: [[1, 'desc']]
    
    #block name is specified here: see more in Components/fields.html.twig
    default_list_fields:
        invoice:
            - ID_list
            - IssueDate
            - LegalMonetaryTotal_TaxExclusiveAmount
            - LegalMonetaryTotal_TaxInclusiveAmount
        delivery_note:
            - ID_list
            - IssueDate
        order:
            - ID_list
            - OrderReference_IssueDate
            - LegalMonetaryTotal_TaxExclusiveAmount
            - LegalMonetaryTotal_TaxInclusiveAmount
        credit_memo:
            - ID_list
            - IssueDate
            - LegalMonetaryTotal_TaxExclusiveAmount
            - LegalMonetaryTotal_TaxInclusiveAmount
    
    default_detail_fields:
        invoice:
            - Item_SellersItemIdentification_ID
            - Item_Name
            - InvoicedQuantity
            - SesExtension_UnitPrice
            - Price_PriceAmount
            - Sum
        delivery_note:
            - Item_SellersItemIdentification_ID
            - Item_Name
            - InvoicedQuantity
            - SesExtension_UnitPrice
        order:
            - Item_SellersItemIdentification_ID
            - Item_Name
            - InvoicedQuantity
            - SesExtension_UnitPrice
            - Price_PriceAmount
            - Sum
        credit_memo:
            - Item_SellersItemIdentification_ID
            - Item_Name
            - InvoicedQuantity
            - SesExtension_UnitPrice
            - Price_PriceAmount
            - Sum
```

## FAQ

How to configure columns in a list view?

You can configure which columns (per document type) you want to display. The columns in the *list* are sortable.

The column identifier is the block name from the *views/OrderHistory/Components/fields.html.twig* without the suffix '\_field'. [Read more...](https://doc.silver-eshop.de/display/EZC14/Orderhistory+-+Templates)

``` 
siso_order_history:
    default_list_fields:
        invoice:
            - ID_list
            - IssueDate
            - LegalMonetaryTotal_TaxExclusiveAmount
            - LegalMonetaryTotal_TaxInclusiveAmount 

    default_detail_fields:
        invoice:
            - Item_SellersItemIdentification_ID
            - Item_Name
            - InvoicedQuantity
            - SesExtension_UnitPrice
            - Price_PriceAmount
            - Sum
```

How to change sorting for the columns in a list view?

You can configure how the columns (per document type) in the list view are sorted. Check the [DataTables plugin configuration](https://www.datatables.net/plug-ins/sorting/) to see all sorting options.

For the date it is enough to use the 'datetime' sorting. Following date-formats are supported:

    #combination of these characters is allowed: Y,m,d
    #accepted delimiters are: "/" , "." and "-"
    #Example: d.m.Y, Y/m/d, d-m-Y...

``` 
siso_order_history:
    default_list_sort:
        invoice:
            - numeric-comma
            - datetime
            - numeric-comma
            - numeric-comma
    
```

You can also configure (per document type) by which column the list will be sorted by default:

``` 
siso_order_history:
    # default column sorting for list view
    # valid values: [[<column_index>,<sort_order>], ... , [<column_index>,<sort_order>]]
    # column_index of first column is 0!
    default_list_sort_column:
        #example multiple sorting
        #invoice: [[0, 'desc'], [2, 'asc']]
        invoice: [[1, 'desc']]
        
```

How to change the date format?

 You can configure the ERP date format and also the date format that will be used in the shop:

``` 
parameters:
    #date format that is used and displayed in shop
    #combination of these characters is allowed: Y,m,d
    #accepted delimiters are: "/" , "." and "-"
    #Example: d.m.Y, Y/m/d, d-m-Y...
    siso_order_history.default.date_format: 'd.m.Y'

    #date format that is used for communication with ERP
    siso_order_history.default.erp_date_format: 'Ymd' 
```

How to change the configuration by siteaccess?

The semantic configuration (orderhistory.yml) is read and prepared in the ConfigurationReaderService. However an event is dispatched here, that allows you to modify the configuration, if you e.g. need to change it by siteaccess.

**How the semantic configuration is used?** Expand source 

``` 
# vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Services/ConfigurationReaderService.php
# check the orderhistory.yml
$list = %siso_order_history.list%;
$document_types = %siso_order_history.document_types%;
$default_document_type = %siso_order_history.default_document_type%;
$date = %siso_order_history.date%;
$default_list_fields = %siso_order_history.default_list_fields%;
$default_list_sort = %siso_order_history.default_list_sort%;
$default_list_sort_column = %siso_order_history.default_list_sort_column%;
$default_detail_fields = %siso_order_history.default_detail_fields%;

...
$data = array(
    'list' => $list,
    'document_types' => $document_types,
    'default_document_type' => $default_document_type,
    'date' => $this->dateTimeService->formatDateAmountToDate($date),
    'default_list_fields' => $default_list_fields,
    'default_list_sort' => $default_list_sort,
    'default_list_sort_column' => $default_list_sort_column,
    'default_detail_fields' => $default_detail_fields
);

...

/**
 * Dispatch the event ConfigurationEvents::READ_CONFIGURATION
 * The purpose of this event is to have a possibility to change the existing configuration depending on the siteAccess.
 *
 * @return void
 */
protected function getConfigurationData()
{
    /** @var  ReadConfigurationEvent $event */
    $event = new ReadConfigurationEvent();
    $event->setData($this->data);

    $this->eventDispatcher->dispatch(
        ConfigurationEvents::READ_CONFIGURATION,
        $event
    );

    $this->data = $event->getData();
}
```

Therefore you need to implement a '*siso\_order\_history.read\_configuration*' listener.

Add a configuration by siteaccess:

``` 
parameters:
   siso_order_history.default.document_types:
       - order
   siso_order_history.default.default_document_type: order
```

Implement an event listener:

``` 
public function onReadConfiguration(ReadConfigurationEvent $readConfigurationEvent)
{
    $data = $readConfigurationEvent->getData();

    $data['document_types'] = $this->configResolver->getParameter('document_types', 'siso_order_history');
    $data['default_document_type'] = $this->configResolver->getParameter(
                'default_document_type', 
                'siso_order_history'
   );

   $readConfigurationEvent->setData($data);
}

<service id="project.read_configuration_listener" class="%project.read_configuration_listener.class%">
    <argument type="service" id="ezpublish.config.resolver"/>
    <tag name="kernel.event_listener"  event="siso_order_history.read_configuration" method="onReadConfiguration" />
</service>
```

## Templating

The orderhistory is using a list of templates which can be overridden if required. 

[Read more...](Orderhistory---Templates_23560621.html)

## Cookbook

Find recipes in [cookbook...](Orderhistory---Cookbook_23560619.html)

## API

The [API section ](Orderhistory---API_23560628.html)describes how to extend the orderhistory. 

The section [ERP-Messages](Orderhistory---ERP-messages_23560480.html) describes which messages are used towards the ERP system. 
