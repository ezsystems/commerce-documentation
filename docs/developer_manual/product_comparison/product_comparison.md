#  Product comparison 

## Introduction

eZ Commerce offers feature to compare desired products on a single page. Customers can save products on their own comparison lists. The product will be saved on comparison list depending on it's type (like speakers, micrphones). 

Additionally there is a functionality for displaying "Public Comparison List". It's a special comparison list, which can be set by the shop administrator, for which the customers can just view them and are not allowed to modify the list. It's very useful for showing fixed comparison list (eg. top microphones) by placing it's link on the website. 

This feature is available **for all** the customers ( both logged and anonymous ).

The comparison function is based on the basket system of eZ Commerce. 

## Before you start 

Please keep in mind that Comparison of Products is really connected with a lot of different modules in our shop. Be sure to check these out:

  - [Basket](http://confluence.ng.silverproducts.de/display/EX/Basket)
  - [BasketService and BasketCommand](http://confluence.ng.silverproducts.de/display/EX/BasketService+and+BasketCommand)

## Features

The comparison function offers a user friendly way to compare products: [Explore the features](Product-comparison---Features_23560678.html).

## FAQ

How to specify logic for categories

In order to change the logic of determining to which categories should be assigned to the particular products, there is a need to override or implement new comparison service.

First of all the service must implement "[ComparisonServiceInterface](https://doc.silver-eshop.de/display/EZC14/Product+comparison+-+API)". There is a method, called getComparisonCategory(), which gets a CatalogElement (thus also products) passed as an argument. This method must implement the logic for the determination of a  comparison category for the passed product.

Our standard logic implementation:

1.  Try to take product type from CatalogElement
2.  Try to take it from parent element category
3.  If all fails put it to default category

Where are product attributes stored for comparison lists

Product attributes, that are used for comparison, come from CatalogElement. The standard implementation of the eZ Commerce stores CatalogElements in the eZ Platform CMS.

There are 2 types of information:

<table>
<thead>
<tr class="header">
<th>Type</th>
<th>Attribute Names</th>
<th>Features</th>
</tr>
</thead>
<tbody>
<tr>
<td>Dynamic fields from dataMap attribute</td>
<td><p>manufacturer</p>
<p>manufacturerSku</p>
<p>color</p></td>
<td><p>Those attributes are stored in dataMap. They are part of Technical Information group.</p>
<p>Manufacturer, ManufacturerSku and Color is handled without eZ matrix. The names that should be read from the dataMap must be configured in the parameter: siso_comparison.&lt;default&gt;.technical_attributes</p></td>
</tr>
<tr>
<td>Dynamic fields from specification attribute</td>
<td>specification</td>
<td><div class="content-wrapper">
<p>See <a href="https://doc.silver-eshop.de/display/EZC14/SpecificationsType">SpecificationsType</a></p>
<p><br />
</p>
</td>
</tr>
</tbody>
</table>

How collapse of groups and attributes works

It can happen that in one comparison list you have many products and one of the attributes is the same for every product.

It means that you show a row in the table, which have the same value. eZ Commerce have different possibilities to handle that kind of situation.

#### Show/Hide attributes for the same value

There is an icon in top left corner to show/hide attributes. If set to true hiding/showing same attributes is enabled.

``` 
# each attribute that have the same values for each product can be hidden
siso_comparison.default.collapse_attributes: true
```

#### Collapsing all group for product attributes

There is a parameter that defines if the groups with all identical values should be collapsed or not (by default). 

This means that all rows inside a group are identical.

``` 
# behaviour for groups were all attributes have the same values
siso_comparison.default.collapse_groups: true
```

How sorting works with drag & drop 

We need to divide this into 2 lists: private (for customer) and public.

<table>
<thead>
<tr class="header">
<th>Public List</th>
<th>Private List</th>
</tr>
</thead>
<tbody>
<tr>
<td><p>For public lists user can use drag &amp; drop functionality, but it will not be saved.</p>
<p>Only administrator is able to change the order.</p>
<p>It means that visually it will be moved by not stored. When page is refreshed order will remain.</p></td>
<td><p>For private lists when drag and drop is done, ajax call is triggered with new sort order for the chosen list. </p>
<p>Sorting functionality is done by basket line's attribute "<strong>groupOrder</strong>". It is a part of BasketLine entity.</p></td>
</tr>
</tbody>
</table>

How to control which system shall provide prices and stock information

Using a configuration parameter it can be defined which system shall provide the prices and stock infos.

By default the ERP will be requested if it is not available the local price provider will respond. 

``` 
siso_price.default.price_service_chain.comparison:
        - siso_price.price_provider.remote
        - siso_price.price_provider.local
```

## Templating

### Templates list:

Default path:

    vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Path
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>Comparison/comparison_list.html.twig</code></pre></td>
<td>entry page for comparison list</td>
</tr>
<tr>
<td><pre><code>Basket/messages.html.twig</code></pre></td>
<td>template with success/error/notice messages for baskets</td>
</tr>
<tr>
<td><pre><code>parts/user_menu.html.twig</code></pre></td>
<td>html content for right user menu navigation with links to comparison</td>
</tr>
</tbody>
</table>

### Related custom Twig modifiers/functions/etc/:

#### Twig functions

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Twig function
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
<th><div class="tablesorter-header-inner">
Usage
</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>ses_comparison_category</code></pre></td>
<td><p>Returns correct comparison category for the catalog element. This is a wrapper for <a href="https://doc.silver-eshop.de/display/EZC14/Product+comparison+-+API#Productcomparison-API-Service">ComparisonServiceInterface::getComparisonCategory()</a></p></td>
<td><pre><code>{{ ses_comparison_category(catalogElement) }}</code></pre></td>
</tr>
</tbody>
</table>

## API

### Basket type

Comparison is a [Basket](http://confluence.ng.silverproducts.de/display/EX/Entities) with a special type '*comparison*'. 

When adding items no Events are thrown, therefore from the performance perspective it is quicker than adding items into basket. However, there is no data validation in the background, so it is allowed to mix different products. So the data validation, like checking the minimum order amount or checking the mixing of downloads with normal products, is done when adding those items into cart.

``` 
// \Silversolutions\Bundle\EshopBundle\Services\BasketService
const TYPE_COMPARISON = 'comparison';
```

#### Additional Attributes

In order to handle the necessary additional data for the comparison list, new attributes are added to the basket instances of type '*comparison*'. These attributes are **not declared** in the class [Basket](http://confluence.ng.silverproducts.de/display/EX/Entities) , but must be **added dynamically** by the ComparisonServiceInterface implementation.

##### $comparisonAttributes

This is an associative array, which defines the comparable attributes for the current object's comparison category. The sub-structure looks as following:

``` 
array(
    'Group Name' => array( // attribute group (e.g. 'Technical information')
        'list' => array( // static array key
            array (
                'name' => (string) '', // label of the comparison attribute
                'priority' => (int) 0, // index of order for the comparison attribute list
            ),
        ),
        // [...]
    ),
    // [...]
)
```

##### $comparisonElements

 This is an array, which contains the data for the product columns in the comparison list. The sub-structure looks as following:

``` 
array(
    array(
        'catalogElement' => (ProductNode) object, // the respective CatalogElement / ProductNode for the current column
        'attributes' => array(), // same structure as $comparisonAttributes, except that 'name' contains the attribute's value instead of the label
        'basketLineId' => (int) 0, // the ID of the respective (comparison-)basket line (product column in the comparison list)
    ),
    // [...]
)
```

### Service

Comparison feature has an interface, which determines methods necessary to implement different comparison services. 

``` 
interface ComparisonServiceInterface {

    // Determines the correct comparison category for the given catalog element.
    //
    // returns The category string
    public function getComparisonCategory(CatalogElement $catalogElement);

    // This method determines the necessary comparison information for every given comparison category.
    //
    // It accepts an array of Baskets with the type BasketService::TYPE_COMPARISON. Every basket represents one
    // comparison category. All Baskets in the list are expanded dynamically by additional object attributes:
    // $comparisonAttributes
    // $comparisonElements
    // 
    // These object attributes contain the following information:
    // - comparison attributes and their values for products
    // - additionally which groups or attributes should be collapsed
    //
    // return The given array of Baskets with the additional attributes.
    public function getComparisonInformation(array $comparisonList);
}
```
