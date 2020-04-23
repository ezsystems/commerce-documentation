# Product category UI

eZ Commerce allows to show a product category page using different layouts:

``` 
#possible values: product_list, categories, both
siso_core.default.category_view: product_list
```

The categories are rendered using a template `Catalog/Subrequests/listChildren.html.twig`

Offered options: 

- `product_list`: display product list directly
- both: display subcategories and product list - here we may display the facets in the left sidebar instead of the navigation.
- categories:  display only subcategories and only on the last category (if there are no subcategories) display the product list.

## Data provided for categories

Please see [`CatalogElement`](../catalog_api/product_category_catalogelement.md) and `CatalogNode` for a detailed documentation.

### Product category (CatalogElement)

**Predefined properties for `CatalogElement`**

Each `CatalogElement` has predefined properties. These methods are validated automated on constructor by the `validateProperties()` method.

||||
|--- |--- |--- |
|Identifier|Type|Description|
|name|string|The name of the catalog|
|text|string|A short introduction text for the catalog|
|image|ImageField (FieldInterface)|An image for the catalog|
|path|array|The path of the catalog (array of identifier)|
|url|string|The internal URL of the catalog. This url should not be used for generating a link! Please use seoUrl instead|
|seoUrl|string|The human readable URL of the category|
|permanentUrl|string|The internal permanent URL|
|parentElementIdentifier|string|The unqique identifier of the parent catalog|
|identifier|string|The unqique identifier|
|dataMap|FieldInterface[]|A list of Field objects|
|cacheIdentifier|int|string|cache identifier of element to use as key in cache storage|

There are 4 public methods to set properties: 

- setImage, 
- setName, 
- setText, 
- setCacheIdentifier
