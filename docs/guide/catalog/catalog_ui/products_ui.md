# Products UI

## Data provided for products

In addition to the list of Fields provided for a catalog, additional Fields are provided for each product.

## Additional fields within the dataMap

`dataMap` provides additional fields within some properties.
The `dataMap` provides an array of objects which implements the `FieldInterface`.
The Twig function `ses_render_field()` can automatically use either a property of the `CatalogElement`, or a property from the `dataMap`.
If you have a property within the `dataMap` which has the same name as another property of the `CatalogElement`,
you can enforce using the property from the `dataMap` by using the `enforce_datamap` parameter.

For example:

Object `$catalogElement` has property `price` (which has a `PriceField` with value `1.00`) and a `PriceField` `price` (value `2.00`) within its `dataMap`.

``` html+twig
{# "1.00" is output #}
{{ ses_render_field(catalogElement, 'price') }}
 
{# "2.00" is output #}
{{ ses_render_field(catalogElement, 'price', {"enforce_datamap": true}) }}
```

## Attributes of ProductNode

`ProductNode` offers attributes which can be used in templates. They are stored in `$dataMap` and can be used with `{{ ses_render_field() }}`.

In addition, a `ProductNode` provides the attributes of a [`CatalogElement`](../catalog_api/product_category_catalogelement.md).

|Identifier|Type|Description|Usage|
|--- |--- |--- |--- |
|`manufacturerSku`|string|SKU of the product from the manufacturer|`{{ ses_render_field(catalogElement, 'manufacturerSku') }}`|
|`specification`|text block|technical specification|`{{ ses_render_field(catalogElement, 'specification') }}`|
|`manufacturer`|string||`{{ ses_render_field(catalogElement, 'manufacturer') }}`|
|`color`|string|choosed color|`{{ ses_render_field(catalogElement, 'color') }}`|
|`video`|string|the URL for the video|`{{ ses_render_field(catalogElement, 'video') }}`|
|`image1`|image|optional image|`{{ ses_render_field(catalogElement, 'image1') }}`</br>`{{ catalogElement.dataMap.image1 }}`</br>`<img src="/{{ catalogElement.dataMap.image1.path }}" />`|
|`image2`|image|optional image|`{{ ses_render_field(catalogElement, 'image2') }}`</br>`{{ catalogElement.dataMap.image2 }}`</br>`<img src="/{{ catalogElement.dataMap.image2.path }}" />`|
|`image3`|image|optional image|`{{ ses_render_field(catalogElement, 'image3') }}`</br>`{{ catalogElement.dataMap.image3 }}`</br>`<img src="/{{ catalogElement.dataMap.image3.path }}" />`|
|`image4`|image|optional image|`{{ ses_render_field(catalogElement, 'image4') }}`</br>`{{ catalogElement.dataMap.image4 }}`</br>`<img src="/{{ catalogElement.dataMap.image4.path }}" />`|
|`specification`|array|technical specifications|`{{ ses_render_field(catalogElement, 'specification') }}`|

## Get a ProductNode by SKU

If you need to fetch a product in template (e.g. in basket), use the Twig function `ses_product`.

``` html+twig
{% set product = ses_product({'sku': 1100}) %}
SKU: {{ product.sku }}
Name: {{ product.name }}
Image: {{ ses_render_field(product, 'image1') }}
```

Parameters:

|Parameter|Required|Description|
|--- |--- |--- |
|`sku`|yes|The SKU of the product node|
|`sub_tree_path`|no|An optional subtree path, to search product nodes (default: `/1/2` to search in the whole tree)|
|`variantCode`|no|An optional parameter if a given variant is returned|

## Images and assets

Images and assets can be taken from the content model (if this data provider is used).
External images can also be stored in the file system. 

If images come from the file system, they have to be stored in the following folders (the folders can be configured):

- Product images in `web/var/assets/product_images`
- Product group images `web/var/assets/product_group_images`

To avoid too many files in one directory, subfolders are used.

For example, images for a produkt with SKU `D4241`, are stored in the file system in:

``` 
web/var/assets/product_images/D/4/D4142_picture.jpg
web/var/assets/product_images/D/4/D4142_picture_2.jpg
```

When using a cluster, this folder has to be mounted as an NFS shared or cluster FS file system. 

These folders are used as source folders for all assets.

Images are resized automatically when they are used for the first time.
The different image variations are stored in `web/var/ecommerce/storage/<image_class>/<subfolders>`:

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

Image paths are cached in order to avoid accessing the file system in production mode. The cache uses stash. 

### Remove stash cache for images and one SKU

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
