# Products UI

## Data provided for products

In addition to the list of fields provided for a catalog, additional fields are provided for each product. Please see `Product category (CatalogElement)` and `ProductNode` for a detailed documentation.

### ProductNode & OrderableProductNode

Predefined properties for ProductNode

Each `ProductNode` has predefined properties. These methods are validated automated on constructor by the `validateProperties()` method.

|Identifier|Identifier</br>eZ|Type|Description|
|--- |--- |--- |--- |
|name|ses_name|string|Product name|
|sku|ses_sku|string|Unique Stock Keeping Unit (SKU) of a product|
|manufacturerSku|ses_manufacturer_sku|string|Stock Keeping Unit (SKU) by a manufacturer of a product|
|ean|ses_ean|string|European Article Number (EAN) of a product|
|type|ses_product_type|string|Type of a product, e.g. vegetable|
|isOrderable||boolean|True, if a product is orderable|
|price|ses_unit_price|FieldInterface|Price of a product|
|customerPrice||PriceField (FieldInterface)|Customer price of a product which might be generated from a PriceProvider|
|scaledPrices||ArrayField|An array with scaled prices and parameters to determine which scale price should be applied|
|stock|ses_stock_numeric|FieldInterface|Returns the stock of  a product|
|subtitle|ses_subtitle|TextBlockField (FieldInterface)||
|shortDescription|ses_short_description|TextBlockField (FieldInterface)|Short description of a product|
|longDescription|ses_long_description|TextBlockField (FieldInterface)|Long description of a product|
|specifications|ses_specifications|FieldInterface[]|A list of specifications of a product|
|imageList|ses_image_1 .. ses_image_4|ImageField[] (FieldInterface[])|A list of images|
|minOrderQuantity|ses_min_order_quantity|float|Minimal order quantity of the product|
|maxOrderQuantity|ses_max_order_quantity|float|Maximal order quantity of the product|
|allowedQuantity||string|Regex that indicates the allowed quantity</br>this regex can be evaluated using the preg_match() function by some processes to check if the given quantity corresponds to the allowed quantity</br>values of ProductNodeConstants::ALLOWED_QUANTITY_* can be used</br>Possible constants:</br>ALLOWED_QUANTITY_INTEGER</br>ALLOWED_QUANTITY_UP_TO_ONE_DECIMAL_PLACE</br>ALLOWED_QUANTITY_UP_TO_TWO_DECIMAL_PLACES</br>ALLOWED_QUANTITY_MULTIPLE_DECIMAL_PLACES</br>Example for an individual expression: `'#^[0-9]+(\.|,)?[0-9]*$#'`</br>If not set the quantity can be int only!</br>Example:</br>`'allowedQuantity' =>  ProductNodeConstants::ALLOWED_QUANTITY_UP_TO_ONE_DECIMAL_PLACE,`|
|packagingUnit|ses_packaging_unit|float|Packaging unit of the product|
|unit|ses_unit|string|A unit of the product|
|vatCode|ses_vat_code|string|VAT code of the product. Needed to determine VAT percent|

For every property there is a public setter method

## Additional fields within the dataMap

In addition the fields within some properties for each catalog additional fields will be provided in the property `dataMap`. The `dataMap` will provide an array of objects which implements the `FieldInterface`. The Twig function `ses_render_field()` can automatically decide, either to use a property of the `CatalogElement` or a property from the `dataMap`. If you have a property within the `dataMap` which has the same name as another property of the `CatalogElement`, you can enforce using the property from the `dataMap` by using the "`enforce_datamap`" parameter.

Example:

Object `$catalogElement` has property `price` (which has a `PriceField` with value "1.00") and a `PriceField` "price" (value "2.00") within its `dataMap`.

``` 
{# "1.00" is outputted #}
{{ ses_render_field(catalogElement, 'price') }}
 
{# "2.00" is outputted #}
{{ ses_render_field(catalogElement, 'price', {"enforce_datamap": true}) }}
```

## Attributes of ProductNode

Here are some Attributes, that already can be used in the template. These attributes are stored in $dataMap and can be used with {{ ses\_render\_field() }}.

In addition a product node provides the attributes of a CatalogElement (see [Product category](../catalog_api/product_category_catalogelement.md).

|identifier|type|description|usage|
|--- |--- |--- |--- |
|manufacturerSku|string|SKU of the product from manufacturer|`{{ ses_render_field(catalogElement, 'manufacturerSku') }}`|
|specification|text block|Technical specification|`{{ ses_render_field(catalogElement, 'specification') }}`|
|manufacturer|string||`{{ ses_render_field(catalogElement, 'manufacturer') }}`|
|color|string|choosed color|`{{ ses_render_field(catalogElement, 'color') }}`|
|video|string|The url for the video|`{{ ses_render_field(catalogElement, 'video') }}`|
|image1|image|optional image|`{{ ses_render_field(catalogElement, 'image1') }}`</br>`{{ catalogElement.dataMap.image1 }}`</br>`<img src="/{{ catalogElement.dataMap.image1.path }}" />`|
|image2|image|optional image|`{{ ses_render_field(catalogElement, 'image2') }}`</br>`{{ catalogElement.dataMap.image2 }}`</br>`<img src="/{{ catalogElement.dataMap.image2.path }}" />`|
|image3|image|optional image|`{{ ses_render_field(catalogElement, 'image3') }}`</br>`{{ catalogElement.dataMap.image3 }}`</br>`<img src="/{{ catalogElement.dataMap.image3.path }}" />`|
|image4|image|optional image|`{{ ses_render_field(catalogElement, 'image4') }}`</br>`{{ catalogElement.dataMap.image4 }}`</br>`<img src="/{{ catalogElement.dataMap.image4.path }}" />`|
|specification|array|technical specifications|`{{ ses_render_field(catalogElement, 'specification') }}`|

## Get a ProductNode by SKU

If fetching of a product in template is necessary (e.g. in basket), there is the Twig function `ses_product`.

``` 
{% set product = ses_product({'sku': 1100}) %}
SKU: {{ product.sku }}
Name: {{ product.name }}
Image: {{ ses_render_field(product, 'image1') }}
```

Twig `ses_product` parameters

|Parameter|Mandatory|Description|
|--- |--- |--- |
|sku|yes|The SKU of the product node|
|sub_tree_path|no|An optional eZ sub tree path, to search product nodes (default: /1/2 to search within complete tree)|
|variantCode|no|An optional parameter if a given variant shall be returned|

# Images and assets

Images and assets can be taken from the CMS (if the dataprovider eZ is used) or external images can be used stored in the filesystem. 

If images from the file system shall be used they have to be stored in the following folders (the folders can be configured). In order to avoid to many files in one directory subfolders are used located under the following folders

- Product images in `web/var/assets/product_images`
- Product group images `web/var/assets/product_group_images`

Example: 

- SKU:  D4241

Files in the file system: 

``` 
web/var/assets/product_images/D/4/D4142_picture.jpg
web/var/assets/product_images/D/4/D4142_picture_2.jpg
```

When using a cluster this folder has to be mounted as a NFS share ore gluster FS filesystem. 

These folders are used as a source folders for all kind of assets.

Images will be resized automatically when they will be used for the first time. The different image variations will be stored in 

`web/var/ecommerce/storage/<image_class>/<subfolders>`

``` 
web/var/ecommerce/storage//images/image_zoom/D/4/1/-D4142_picture_2_image_zoom.jpg
web/var/ecommerce/storage//images/image_zoom/D/4/1/-D4142_picture_image_zoom.jpg
web/var/ecommerce/storage//images/thumb_big/D/4/1/-D4142_picture_2_thumb_big.jpg
web/var/ecommerce/storage//images/thumb_big/D/4/1/-D4142_picture_thumb_big.jpg
web/var/ecommerce/storage//images/thumb_medium/D/4/1/-D4142_picture_thumb_medium.jpg
web/var/ecommerce/storage//images/thumb_medium/D/4/1/589768-D4142_thumb_medium.jpg
web/var/ecommerce/storage//images/thumb_smaller/D/4/1/-D4142_picture_2_thumb_smaller.jpg
web/var/ecommerce/storage//images/thumb_smaller/D/4/1/-D4142_picture_thumb_smaller.jpg
```

The image paths are cached in order to avoid accessing the filesystem in production mode. The cache is using stash. 

## Remove stash cache for images and one sku

``` php
 /** @var $catalogElement \Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement */
        $catalogElement = $catalogService->getDataProvider()->fetchElementBySku(
            $sku,
            array(),
            $ezHelper->getCurrentLanguageCode()
        );
 $assetService = $this->get('silver_catalog.asset_service');
 $assetService->removeAssetCache($catalogElement);
```
