# Product comparison API

## Basket type

Comparison is a basket with a special type `comparison`. 

When adding items no events are thrown, so adding to comparison is quicker than adding items to a basket.
However, there is no data validation in the background, so you can mix different products.
Data validation, such as checking the minimum order amount or checking the mixing of downloads with normal products,
is done when adding those items into a cart.

## Additional attributes

New attributes are added to `comparison` baskets to handle the necessary additional data for the comparison list.
These attributes are not declared in the class Basket, but must be added dynamically by the `ComparisonServiceInterface` implementation.

### $comparisonAttributes

`$comparisonAttributes` is an associative array which defines the comparable attributes for the current object's comparison category. The sub-structure looks as following:

``` php
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

### $comparisonElements

`$comparisonElements` is an array which contains the data for the product columns in the comparison list. The sub-structure looks as following:

``` php
array(
    array(
        'catalogElement' => (ProductNode) object, // the respective CatalogElement / ProductNode for the current column
        'attributes' => array(), // same structure as $comparisonAttributes, except that 'name' contains the attribute's value instead of the label
        'basketLineId' => (int) 0, // the ID of the respective (comparison-)basket line (product column in the comparison list)
    ),
    // [...]
)
```

## Service

The `ComparisonServiceInterface` interface determines methods necessary to implement different comparison services.

``` php
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
