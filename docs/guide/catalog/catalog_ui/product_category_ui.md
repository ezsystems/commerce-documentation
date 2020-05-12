# Product category UI

You can display a product category page using different layouts:

``` yaml
#possible values: product_list, categories, both
siso_core.default.category_view: product_list
```

The categories are rendered using `Catalog/Subrequests/listChildren.html.twig`.

Available options: 

- `product_list`: display product list directly
- `both`: display subcategories and product list, including an option to display facets in the left sidebar instead of the navigation
- `categories`:  display only subcategories and only on the last category (if there are no subcategories) display the product list

## Data provided for categories

See [CatalogElement](../catalog_api/product_category_catalogelement.md) and `CatalogNode` for more details.
