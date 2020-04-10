# Orderhistory FAQ

## How to configure columns in a list view?

You can configure which columns (per document type) you want to display. The columns in the *list* are sortable.

The column identifier is the block name from the *views/OrderHistory/Components/fields.html.twig* without the suffix '\_field'. [Read more...](orderhistory_templates.md)

``` yaml
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

## How to change sorting for the columns in a list view?

You can configure how the columns (per document type) in the list view are sorted. Check the [DataTables plugin configuration](https://www.datatables.net/plug-ins/sorting/) to see all sorting options.

!!! note

    For the date it is enough to use the 'datetime' sorting. Following date-formats are supported:

    ```
    #combination of these characters is allowed: Y,m,d
    #accepted delimiters are: "/" , "." and "-"
    #Example: d.m.Y, Y/m/d, d-m-Y...
    ```

``` yml
siso_order_history:
    default_list_sort:
        invoice:
            - numeric-comma
            - datetime
            - numeric-comma
            - numeric-comma
    
```

You can also configure (per document type) by which column the list will be sorted by default:

``` yaml
siso_order_history:
    # default column sorting for list view
    # valid values: [[<column_index>,<sort_order>], ... , [<column_index>,<sort_order>]]
    # column_index of first column is 0!
    default_list_sort_column:
        #example multiple sorting
        #invoice: [[0, 'desc'], [2, 'asc']]
        invoice: [[1, 'desc']]
        
```

## How to change the date format?

You can configure the ERP date format and also the date format that will be used in the shop:

``` yaml
parameters:
    #date format that is used and displayed in shop
    #combination of these characters is allowed: Y,m,d
    #accepted delimiters are: "/" , "." and "-"
    #Example: d.m.Y, Y/m/d, d-m-Y...
    siso_order_history.default.date_format: 'd.m.Y'

    #date format that is used for communication with ERP
    siso_order_history.default.erp_date_format: 'Ymd' 
```

## How to change the configuration by siteaccess?

The semantic configuration (`orderhistory.yml`) is read and prepared in the `ConfigurationReaderService`. However an event is dispatched here, that allows you to modify the configuration, if you e.g. need to change it by siteaccess.

How the semantic configuration is used?:

``` yaml
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

Therefore you need to implement a `siso_order_history.read_configuration` listener.

Add a configuration by siteaccess:

``` yaml
parameters:
    siso_order_history.default.document_types:
       - order
    siso_order_history.default.default_document_type: order
```

Implement an event listener:

``` php
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
```

``` xml
<service id="project.read_configuration_listener" class="%project.read_configuration_listener.class%">
    <argument type="service" id="ezpublish.config.resolver"/>
    <tag name="kernel.event_listener"  event="siso_order_history.read_configuration" method="onReadConfiguration" />
</service>
```
