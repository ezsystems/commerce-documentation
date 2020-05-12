# Content model data provider

The content model data provider provides an implementation for fetching catalogues and products from the database.

Products are stored directly in the content structure.
They can use the features provided by the content model such as languages, Objects states, versioning, etc.
The product catalog can be maintained in the Back Office.

## Requirements

Two content classes have to be installed in the system:

- Category
- Product

### Catalog Content Type

The following Fields are used to create a `CatalogElement`:

- name: `ses_name`
- text: `ses_subtitle`
- image: `ses_image`
- identifier: <main Node ID>

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

### Product Content Type

The following Fields are used to create a product:

- name: `ses_name`
- text: `ses_subtitle`
- image: `ses_image_main`
- shortDescription: `ses_short_description`
- longDescription: `ses_long_description`
- sku: `ses_sku`
- manufacturerSku: `ses_vendor_sku`
- ean: `ses_ean`
- identifier: <main Node ID>

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

To explicitly fetch a specific translation of a Content item, use the optional `$languages` parameter, for example:

``` php
$seoUrl = '/silver.catalog/Products/Vegetables';
$languages = array('eng-US');
 
/** @var $catalogService \Silversolutions\Bundle\EshopBundle\Services\CatalogService */
$catalogService = $this->get('silver_catalog.data_provider_service');
/** @var $catalogData \Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement */
$catalogData = $catalogService->getDataProvider()->lookupByUrl($seoUrl, $languages);
```

During `lookupByUrl()`, if the catalog element you are searching for has Location set to `invisible`,
the application throws a `HiddenCatalogElementException`. It is not possible to see hidden elements

## Configuration

The data provider uses configuration to limit the fetched catalog elements.

The following default setup filters content of type `ses_category` for the navigation service
(which uses this data provider to fetch the category objects).
The sort order is controlled by the Priority field and the publish date.

The second level key defines the scope, or `filterType`, for which the specific filter definitions are valid.
In this example it is `navigation`, which is passed by the navigation service's fetch.

``` yaml
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

!!! note "Important"

    If a product has multiple Locations, the shop ensures that the proper Location is returned,
    so the URL of the product is correct.

    By default hidden items (e.g. products) are not fetched.

|Parameter|Description|
|--- |--- |
|contentTypes|Identifierd of the Content Types defined in the system|
|limit|Default limit to be used when no limit is given|
|sortClauses|Clauses for sorting|
