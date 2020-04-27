# Product comparison templates

## Templates list

Default path:

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views`

|Path|Description|
|--- |--- |
|Comparison/comparison_list.html.twig|entry page for comparison list|
|Basket/messages.html.twig|template with success/error/notice messages for baskets|
|parts/user_menu.html.twig|html content for right user menu navigation with links to comparison|


## Related custom Twig modifiers/functions/etc

### Twig functions

#### `ses_comparison_category`

Returns correct comparison category for the catalog element. This is a wrapper for `ComparisonServiceInterface::getComparisonCategory()`

``` html+twig
{{ ses_comparison_category(catalogElement) }}
```
