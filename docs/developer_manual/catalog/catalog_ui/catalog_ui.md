#  Catalog - UI 

## Used fields in the frontend

## Product list

![](attachments/23560463/23562930.png)

<table>
<thead>
<tr class="header">
<th>No</th>
<th>Attribute ProductNode</th>
<th>Note</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>catalogElement.name</td>
<td><br />
</td>
</tr>
<tr>
<td>2</td>
<td>catalogElement.sku</td>
<td><br />
</td>
</tr>
<tr>
<td>3</td>
<td>catalogElement.text</td>
<td><br />
</td>
</tr>
<tr>
<td>4</td>
<td>Price from priceengine</td>
<td><br />
</td>
</tr>
<tr>
<td>5</td>
<td>catalogElement.mainImage</td>
<td><br />
</td>
</tr>
</tbody>
</table>

## Product detailpage

![](attachments/23560463/23562929.png)

<table>
<thead>
<tr class="header">
<th>No</th>
<th>Attribute ProductNode</th>
<th>Note</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>catalogElement.mainImage</td>
<td>The main image or the imageList might come from the filesystem directly</td>
</tr>
<tr>
<td>2</td>
<td>catalogElement.imageList</td>
<td><br />
</td>
</tr>
<tr>
<td>3</td>
<td>catalogElement.sku</td>
<td><br />
</td>
</tr>
<tr>
<td>4</td>
<td>catalogElement.unit</td>
<td><br />
</td>
</tr>
<tr>
<td>4a</td>
<td><pre><code>catalogElement.packagingUnit</code></pre></td>
<td>If not present label is hidden</td>
</tr>
<tr>
<td>4b</td>
<td>Country of Origin</td>
<td>If not present label is hidden</td>
</tr>
<tr>
<td>5</td>
<td>catalogElement.text</td>
<td><br />
</td>
</tr>
<tr>
<td>6</td>
<td>catalogElement.shortDescription</td>
<td><br />
</td>
</tr>
<tr>
<td>7</td>
<td>variants</td>
<td><br />
</td>
</tr>
<tr>
<td>8</td>
<td>catalogElement.longDescription</td>
<td>If not present tab is hidden</td>
</tr>
<tr>
<td>9</td>
<td><p>catalogElement.manufacturer</p>
<p>catalogElement.manufacturerSku</p>
<p>catalogElement.specification</p>
<p>catalogElement.dataMap.color</p></td>
<td>This tab contains several attributes from the product. If a attribute is not present it will be not displayed (inc. the label)</td>
</tr>
<tr>
<td>10</td>
<td>catalogElement.dataMap.video</td>
<td><br />
</td>
</tr>
</tbody>
</table>

# Template list

The templates for the catalog have to be located in the views/Catalog folder. Depending on the "viewtype" different templates have to be provided (configuration see [Templates for catalog](Templates-for-catalog_23560475.html)):

<table>
<thead>
<tr class="header">
<th><p>Path</p></th>
<th><p>Description</p></th>
</tr>
</thead>
<tbody>
<tr>
<td><code>product.html.twig</code></td>
<td><br />
</td>
</tr>
<tr>
<td><code>catalog.html.twig</code></td>
<td><br />
</td>
</tr>
<tr>
<td><code>productType.html.twig</code></td>
<td><br />
</td>
</tr>
</tbody>
</table>

The templates have access to the catalog node provided by the controller.

URLs will be provided by special methods:

  - SEO URL `{{ catalogElement.seoUrl }}`
  - Permantent URL `{{ catalogElement.permanentUrl }}`

Since these URLs contain the prefix defined in the routing table as well the `path()` function of Twig cannot be used\!

*eZ Commerce* provides a special rendering function for Twig templates and data from the catalog.

**Example**

``` 
{{ ses_render_field(catalogElement, 'longDescription') }}
```

The renderer uses predefined templates for each [Field](Fields-for-ecommerce-data_23560470.html). The templates are located in the folder "FieldTypes".

## Configuration for templates

The configuration for choosing templates is stored in a `yml` file `silver.eshop.yml`:

``` 
parameters:
  
  silver_eshop.default.catalog_factory.sis_category: createCatalogNode
  silver_eshop.default.catalog_factory.sis_product: createOrderableProductNode

  silver_eshop.default.catalog_template.CatalogNode: Catalog:catalog.html.twig
  silver_eshop.default.catalog_template.OrderableProductNode: Catalog:product.html.twig
```

# Show all attributes of a CatalogElement

To show all available attributes the method `getAttributeNames()` is given.

``` 
{{ catalogElement.attributeNames|json_encode }}
{# returns a JSON (because of Twig filter "json_encode":
{
    "propertyName": {
        "name": "propertyName",
        "fromDataMap": false,
        "hasValue": bool,
        "type": <type>
    },
    ...
    "dataMap": {
        "dataMapAttributeName": {
            "name": "dataMapAttributeName",
            "type": <realisation of FieldInterface>,
            "fromDataMap": true,
            "hasValue": bool
        },
    }
}
```

## Number of loaded products

You can define the number of products that are loaded in one time.

**silver.eshop.yml**

``` 
#will be loaded in the beginning when the page is rendered for the first time
silver_eshop.default.catalog_product_list_limit: 6
#will be loaded dynamic via ajax later on
silver_eshop.default.catalog_product_list_limit_ajax: 3 
```

  - [Templates for catalog](Templates-for-catalog_23560475.html)
  - [Product category - UI](Product-category---UI_23560292.html)
  - [Products - UI](Products---UI_23560290.html)
  - [Product Variants - UI](Product-Variants---UI_23560478.html)

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-11-9\_14-27-23.png](attachments/23560463/23562930.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-11-9\_14-34-0.png](attachments/23560463/23562929.png) (image/png)  
