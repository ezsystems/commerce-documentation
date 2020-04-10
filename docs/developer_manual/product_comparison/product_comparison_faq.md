# Product comparison FAQ

## How to specify logic for categories

In order to change the logic of determining to which categories should be assigned to the particular products, there is a need to override or implement new comparison service.

First of all the service must implement [ComparisonServiceInterface](product_comparison.md). There is a method, called `getComparisonCategory()`, which gets a CatalogElement (thus also products) passed as an argument. This method must implement the logic for the determination of a  comparison category for the passed product.

Our standard logic implementation:

1. Try to take product type from CatalogElement
1. Try to take it from parent element category
1. If all fails put it to default category

## Where are product attributes stored for comparison lists

Product attributes, that are used for comparison, come from CatalogElement. The standard implementation of the eZ Commerce stores CatalogElements in the eZ Platform CMS.

There are 2 types of information:

|Type|Attribute Names|Features|
|--- |--- |--- |
|Dynamic fields from dataMap attribute|manufacturer</br>manufacturerSku</br>color|Those attributes are stored in dataMap. They are part of Technical Information group.</br>Manufacturer, ManufacturerSku and Color is handled without eZ matrix. The names that should be read from the dataMap must be configured in the parameter: siso_comparison.<default>.technical_attributes|
|Dynamic fields from specification attribute|specification|See SpecificationsType|


## How collapse of groups and attributes works

It can happen that in one comparison list you have many products and one of the attributes is the same for every product.

It means that you show a row in the table, which have the same value. eZ Commerce have different possibilities to handle that kind of situation.

#### Show/Hide attributes for the same value

There is an icon in top left corner to show/hide attributes. If set to true hiding/showing same attributes is enabled.

``` yaml
# each attribute that have the same values for each product can be hidden
siso_comparison.default.collapse_attributes: true
```

#### Collapsing all group for product attributes

There is a parameter that defines if the groups with all identical values should be collapsed or not (by default). 

This means that all rows inside a group are identical.

``` yaml
# behaviour for groups were all attributes have the same values
siso_comparison.default.collapse_groups: true
```

## How sorting works with drag & drop 

We need to divide this into 2 lists: private (for customer) and public.

### Public List

For public lists user can use drag & drop functionality, but it will not be saved.

Only administrator is able to change the order.

It means that visually it will be moved by not stored. When page is refreshed order will remain.

### Private List

For private lists when drag and drop is done, ajax call is triggered with new sort order for the chosen list. 

Sorting functionality is done by basket line's attribute "groupOrder". It is a part of BasketLine entity.

## How to control which system shall provide prices and stock information

Using a configuration parameter it can be defined which system shall provide the prices and stock infos.

By default the ERP will be requested if it is not available the local price provider will respond. 

``` yaml
siso_price.default.price_service_chain.comparison:
        - siso_price.price_provider.remote
        - siso_price.price_provider.local
```
