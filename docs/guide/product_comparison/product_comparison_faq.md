# Product comparison FAQ

## Category logic

To change the logic that determines to which categories to assign particular products,
you need to override the comparison service or implement a new one.

First, the service must implement `ComparisonServiceInterface`.
The `getComparisonCategory()` method takes a CatalogElement (including products) as an argument.
This method must implement the logic to determine the comparison category for the passed product.

The standard logic implementation:

1. Tries to take product type from CatalogElement
1. Tries to take it from parent element category
1. If everything else fails, places the product in the default category

## Storing product attributes

Product attributes that are used for comparison come from CatalogElement.
The standard implementation of the eZ Commerce stores CatalogElements in the content model.

There are two types of information:

|Type|Attribute Names|Features|
|--- |--- |--- |
|Dynamic fields from the `dataMap` attribute|`manufacturer`</br>`manufacturerSku`</br>`color`|These attributes are stored in `dataMap`. They are part of the Technical Information group.</br>Manufacturer, ManufacturerSku and Color are handled without eZ matrix. The names that should be read from `dataMap` must be configured in the `siso_comparison.<default>.technical_attributes` attribute|
|Dynamic fields from specification attribute|`specification`|See [SpecificationsType](../../api/field_type_reference/specificationstype.md)|

## Collapsing groups and attributes

If all products in a comparison list have the same attribute value, 
eZ Commerce can handle this in two ways.

### Show/Hide attributes for the same value

There is an icon in top left corner to show/hide attributes.
If `collapse_attributes` is set to true, hiding/showing same attributes is enabled.

``` yaml
# each attribute that have the same values for each product can be hidden
siso_comparison.default.collapse_attributes: true
```

### Collapsing all group for product attributes

The `collapse_groups` parameter defines if groups with all identical values should be collapsed or not (by default). 

This applies to a situation where all rows inside a group are identical.

``` yaml
# behaviour for groups were all attributes have the same values
siso_comparison.default.collapse_groups: true
```

## Drag and drop sorting

Drag and drop works differently for public and private (customer-specific) lists.

### Public Lists

In public lists the customer can use drag and drop, but the order is not saved.

Only the administrator is able to change the order permanently.

This means that after the page is refreshed, the initial order is displayed.

### Private Lists

For private lists when drag and drop is done, an AJAX call is triggered with new sort order for the list.

Sorting is done by basket line's `groupOrder` attribute. It is a part of the BasketLine object.

## Providing prices and stock information

You can configure which system provides the prices and stock information for comparisons.

By default the ERP is requested. If it is not available, the local price provider responds. 

``` yaml
siso_price.default.price_service_chain.comparison:
    - siso_price.price_provider.remote
    - siso_price.price_provider.local
```
