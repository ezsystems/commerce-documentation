#  Product comparison - API 

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
