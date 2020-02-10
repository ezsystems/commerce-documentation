#  CMS dataprovider 

The Dataprovider for eZ Platform version provides an implementation for fetching catalogues and products from the CMS.

  - products are stored directly in eZ Platform
  - they can use the features provided by the CMS such as languages, objects states, versioning, ..
  - The product catalog can be maintained in the CMS

Requirements

Two Content classes have to be installed in the CMS:

  - Category
  - Product

### The catalogue content type

The following fields are used to create a catalogElement:

  - name: ses\_name
  - text: ses\_subtitle
  - image: ses\_image
  - identifier: main Node-Id

``` 
+------------------------------------------------+
I list attributes of class: ses_category         |
+-----------------------+-----+------------------+
| identifier            | id  | data_type_string |
+-----------------------+-----+------------------+
| ses_name              | 306 | ezstring         |
| ses_subtitle          | 307 | ezstring         |
| ses_short_description | 308 | ezxmltext        |
| ses_long_description  | 309 | ezxmltext        |
| ses_image_main        | 310 | ezimage          |
| ses_code              | 311 | ezstring         |
| ses_meta_keywords     | 312 | ezstring         |
| ses_meta_description  | 313 | ezstring         |
+-----------------------+-----+------------------+
```

### The product content type

The following fields are used to create a product:

  - name: ses\_name
  - text: ses\_subtitle
  - image: ses\_image\_main
  - shortDescription: ses\_short\_description
  - longDescription: ses\_long\_description
  - sku: ses\_sku
  - manufacturerSku: ses\_vendor\_sku
  - ean: ses\_ean
  - identifier: main Node-Id  

``` 
+---------------------------------------------------+
I list attributes of class: ses_product             |
+--------------------------+-----+------------------+
| identifier               | id  | data_type_string |
+--------------------------+-----+------------------+
| ses_name                 | 314 | ezstring         |
| ses_type                 | 315 | ezselection      |
| ses_short_description    | 316 | ezxmltext        |
| ses_long_description     | 317 | ezxmltext        |
| ses_subtitle             | 318 | ezstring         |
| ses_sku                  | 319 | ezstring         |
| ses_ean                  | 320 | ezstring         |
| ses_manufacturer_sku     | 321 | ezstring         |
| ses_unit_price           | 322 | ezstring         |
| ses_image_main           | 323 | ezimage          |
| ses_specifications       | 324 | ezmatrix         |
| ses_variants             | 325 | ezmatrix         |
| ses_unit                 | 326 | ezselection      |
| ses_manufacturer         | 328 | ezstring         |
| ses_color                | 329 | ezstring         |
| ses_specification        | 330 | text block       |
| ses_specification_matrix | 370 | ezmatrix         |
+--------------------------+-----+------------------+
```

## Fetching by language

To explicit fetch a specific translation of an eZ object, you might use the optional `$languages` parameter of the methods.

Example:

``` 
$seoUrl = '/silver.catalog/Products/Vegetables';
$languages = array('eng-US');
 
/** @var $catalogService \Silversolutions\Bundle\EshopBundle\Services\CatalogService */
$catalogService = $this->get('silver_catalog.data_provider_service');
/** @var $catalogData \Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement */
$catalogData = $catalogService->getDataProvider()->lookupByUrl($seoUrl, $languages);
```

During **lookupByUrl** if the catalog element, which we are searching for, has location set to **invisible**, the application will throw **HiddenCatalogElementException**. It is not possible to see hidden elements

## Configuration

The data provider uses a configuration in order to limit the fetched CatalogElements.

The following default setup filters eZ Platform content of type "ses\_category" for the navigation service (which uses this data provider to fetch the category objects). The sort order will be controlled by the Priority field and the publish date.

The second level key defines the scope, or "filterType", for which the specific filter definitions are valid. In this example it is "navigation", which is passed by the navigation service's fetch.

**Example**

``` 
silver_eshop.default.ez5_catalog_data_provider.filter:
        navigation:
            contentTypes: [ "ses_category" ]
            limit: 20
            sortClauses:
                -
                    clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                    order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
                -
                    clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\DatePublished"
                    order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_ASC"
        catalogList:
            contentTypes: ["ses_category", "ses_product"]
            sortClauses:
                -
                    clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                    order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
        productList:
            contentTypes: ["ses_product"]
            sortClauses:
                -
                    clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                    order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
```

Important

In case that a product belongs to multiple locations, the shop ensures that the proper location is returned, so the url of the product is correct.

By default hidden items (e.g. products) are not fetched.

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>contentTypes</code></pre></td>
<td><p>The identifier of the content types defined in the CMS eZ Platform</p></td>
</tr>
<tr>
<td><pre><code>limit</code></pre></td>
<td>Default limit to be used when no limit is given</td>
</tr>
<tr>
<td><pre><code>sortClauses</code></pre></td>
<td>The clauses for sorting.</td>
</tr>
</tbody>
</table>
