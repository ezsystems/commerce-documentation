# Product comparison templates

## Template list

Default path:

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views`

|Path|Description|
|--- |--- |
|`Comparison/comparison_list.html.twig`|Entry page for comparison list|
|`Basket/messages.html.twig`|Template with success/error/notice messages for baskets|
|`parts/user_menu.html.twig`|HTML content for right user menu navigation with links to comparison|

## Related custom Twig functions

### `ses_comparison_category`

Returns correct comparison category for the catalog element.
This function is a wrapper for `ComparisonServiceInterface::getComparisonCategory()`

``` html+twig
{{ ses_comparison_category(catalogElement) }}
```
