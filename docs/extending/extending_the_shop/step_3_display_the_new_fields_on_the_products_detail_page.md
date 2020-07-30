# Step 3 - Display the new fields on the products detail page

!!! note "What we want to achieve"

    In the last chapter we have added 2 new fields:

    - `ses_is_download`
    - `ses_download_file`

    All additional product data is stored in the product dataMap. The dataMap is a flexible attribute, that stores project specific data in an assoziative array. You can access the data from the dataMap any time.

!!! caution

    The new fields are stored in the dataMap using the *camelCase* convention.

## Override the standard template

Please copy the standard template to the new bundle: "src/DemoBundle/Resources/views/Catalog/parts/productData.html.twig"

``` 
cp vendor/silversolutions/silver.e-shop//src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/productData.html.twig  app/Resources/views/Catalog/parts/productData.html.twig
```

Add a new block in order to inform the customer if a product is  a digital product:

**Template example**

``` html+twig
{% if catalogElement.dataMap.isDownload is defined and catalogElement.dataMap.isDownload == true %}
    <p class="u-no-margin">
        <i class="fa fa-arrow-circle-o-down" aria-hidden="true"></i>
        <strong>
            This is a digital product
        </strong>
    </p>
{% endif %}
```

### Writing conventions

To make usage of the flexible product data system, you have to follow the writing conventions

#### Use case: Add new field in eZ

`snake_case` convention

Example: `ses_is_download`

How the data is stored in dataMap:

`camelCase` convention without prefix `ses`

Example: `isDownload`

How to access the data in the template

Directly, or using the `ses_render_field` method

Example:

```html+twig
{% if catalogElement.dataMap.isDownload is defined and catalogElement.dataMap.isDownload == true %}
     Display infos about the download
{% endif %}
```

### Supported eZ fields

Following eZ fields are supported and will be stored automatically in the product dataMap. The shop is not using the eZ fields to be stored in the dataMap, but custom fields. The main reason is, that the shop supports a [flexible catalog system](#) for products stored not only in eZ.

|eZ field|Class|In the dataMap converted to|
|--- |--- |--- |
|Text line (ezstring)|\eZ\Publish\Core\FieldType\TextLine\Value|Silversolutions\Bundle\EshopBundle\Content\Fields\TextLineField|
|Rich text (ezrichtext)|\eZ\Publish\Core\FieldType\XmlText\Value|Silversolutions\Bundle\EshopBundle\Content\Fields\TextBlockField|
|Image (ezimage)|\eZ\Publish\Core\FieldType\Image\Value|Silversolutions\Bundle\EshopBundle\Content\Fields\ImageField|
|BInary (ezbinary)|\eZ\Publish\Core\FieldType\BinaryFile\Value|Silversolutions\Bundle\EshopBundle\Content\Fields\FileField|
|SesSelection (sesselection)|Ibexa/Platform/Commerce/FieldTypes/FieldType/SesExternalData\Value|Silversolutions\Bundle\EshopBundle\Content\Fields\TextLineField|

!!! tip

    If you want to use other eZ fields (e.g. Date and Time), you would have to override the catalog factory.
