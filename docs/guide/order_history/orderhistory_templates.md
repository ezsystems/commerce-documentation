# Orderhistory templates

### Templates list

|Path|Description|
|--- |--- |
|vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/list.html.twig|Renders the list view of requested documents|
|vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/list_table.html.twig|Renders the table with list of requested documents</br>Included in views/OrderHistory/list.html.twig|
|vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/detail.html.twig|Renders the detail view of requested document|
|vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/header_default.html.twig|Renders the header information for document detail</br>Included in views/OrderHistory/detail.html.twig|
|vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/fields.html.twig|Contents blocks that renders content of requested field for columns (defined in the configuration).</br>Included in views/OrderHistory/Components/list_table.html.twig</br>views/OrderHistory/detail.html.twig|
|vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/user_menu.html.twig|See [User menu](../customers/customers_faq.md)|

### Related custom Twig modifiers/functions/etc/:

|Twig filter|Description|Usage|
|--- |--- |--- |
|'ses_erp_to_default'|converts the ERP date into default date format|`{{ 'Order at:'|st_translate }} {{ response.OrderReference.IssueDate.value|ses_erp_to_default }} {{ response.OrderReference.IssueDate.value|ses_erp_to_default }}`|
|'ses_to_float'|converts given value to valid float</br>(also strings that are using comma instead of dot)|`{{ 'VAT:'|st_translate }} {{ vat.TaxAmount.value|ses_to_float|price_format }}`|

### Related routes:

``` yaml
siso_order_history_list:
    path:     /orderhistory
    defaults: { _controller: SisoOrderHistoryBundle:OrderHistory:list }

siso_order_history_detail:
    path:     /orderhistory/detail/{documentType}/{id}
    defaults: { _controller: SisoOrderHistoryBundle:OrderHistory:detail } 
```
