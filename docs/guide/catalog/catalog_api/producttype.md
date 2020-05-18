# ProductType

`ProductType` extends [CatalogElement](product_category_catalogelement.md) and implements `ProductNodeContainerInterface`.

This class defines the methods needed to instantiate a Product Type.

### Properties for ProductType

|Identifier|Type|Description|
|--- |--- |--- |
|`childProducts`|CatalogElement[]|Array of products that belong to this Product Type|
|`shortDescription`|TextBlockField (FieldInterface)|Short description|
|`longDescription`|TextBlockField (FieldInterface)|Long description|
|`specifications`|AbstractField[]|A list of specifications of a product|
|`imageList`|ImageField[] (FieldInterface[])|A list of images|
|`displayInSearch`|bool|`true` if the ProductType should be displayed in the search result|
|`displayInProductList`|bool|`true` if the ProductType should be displayed in the product list result|

### Methods

|Name|Parameters|Return value|Throws|Description|
|--- |--- |--- |--- |--- |
|`getChildProducts`||`CatalogElement[]`||Returns all child products|
|`addChildProducts`|`ProductNode[]`||InvalidArgumentException|Adds the products passed as argument to the list of child products|
|`hasChildProducts`||int||Returns the number of children|
|`getChildProductBySku`|string|CatalogElement||Returns the child product that has the SKU passed as an argument|
|`addChildProduct`|`ProductNode`|||Add a single product to the list of child products|
