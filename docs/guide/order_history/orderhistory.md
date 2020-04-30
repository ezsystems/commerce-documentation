# Orderhistory

## Introduction

Orderhistory is an additional eZ Commerce module that is based on the standard eZ Commerce functionality. The user has the possibilty to see an overview and a details of his orders or other documents from the ERP system. He will see documents based on online purchases as well as order placed e.g. by a telephone call.

On the detail page he can see the buyer, delivery address, ordered items and some more details about the order, like status, delivery and payment selection.

He can also use the addToBasket functionality here and speed up the order process if he e.g. often orders the same items.

Orderhistory supports different document types:

- order
- invoice
- delivery note
- credit memo

## Before you start 

!!! caution "Important"

    Make sure to the users have been granted access to the orderhistory. `siso_policy` `orderhistory_view` must be set.

## Installation

see [Install Order history](order_history_api/install_order_history.md)

## Configuration

Orderhistory is using semantic configuration, so it only exposes parameters that are allowed to be configurable.

However it is possible to override this configuration by siteaccess. Therefore an event is thrown, before the configuration is used and you can implement a listener, that will be able to change this configuration. eZ Commerce makes usage of this event for displaying of the [local orders](order_history_features/orderhistory_local_orders_when_erp_not_available.md). Check our [FAQ](orderhistory_faq.md) to find out, how to implement a new configuration listener.

``` yaml
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
