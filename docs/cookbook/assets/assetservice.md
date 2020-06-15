# AssetService

Instead of storing images or other files (like pdf, exe, zip, etc.) in CatalogElement (dataprovider),
you can store them in a special folder on the web-server (storage).
The file names contain SKU (later with variant code) for assigning the files to the product.

AssetService checks the given strategy and gets all assets for a single catalog element.
AssetServices uses the [StorageService](storageservice.md) to get the assets from the storage.

`AssetService` represents the entry point to the CatalogElement assets. It determines the proper strategy, how the assets are stored in CatalogElement. It may call the StorageService, which gets the assets from the file system and stores all assets for a single CatalogElement.

Service ID: `silver_catalog.asset_service`

## AssetService Functions

|Function|Parameters|Returns|Meaning|TWIG Helper function|
|--- |--- |--- |--- |--- |
|`getAssetsByGroup()`|`CatalogElement $catalogElement`</br>`string $settingsGroup`</br>`string $productId = null`|`array()`|Returns all assets for a single CatalogElement by `settingGroup`. `$productId` is optional, if it isn't set, by default the SKU of `catalogElement` is used.|`{{ses_assets_by_group(catalogElement, 'Manuals', catalogElement.sku) }}`</br>`{{ ses_assets_by_group(catalogElement, 'Manuals') }}`|
|`getMainImage()`|`CatalogElement $catalogElement`</br>`string $productId = null`|FileField or null|Returns the main image for the `catalogElement`. `$productId` is optional, if it isn't set, by default the SKU of `catalogElement` is used.|`{{ ses_assets_main_image(catalogElement, catalogElement.sku) }}`</br>`{{ ses_assets_main_image(catalogElement) }}`|
|`getImageList()`|`CatalogElement $catalogElement`</br>`string $productId = null`|array of FileFields or null|Returns a list of images for the `catalogElement`. `$productId` is optional, if it isn't set, by default the SKU of `catalogElement` is used.|`{{ ses_assets_image_list(catalogElement, catalogElement.sku) }}`</br>`{{ ses_assets_image_list(catalogElement) }}`|

### Usage

``` php
//in the controller
$assetService = $this->get('silver_catalog.asset_service');
$mainImage = $assetService->getMainImage($catalogElement);
```

### Usage inside a Twig template

``` html+twig
{# get the main image #}
{% set mainImage = ses_assets_main_image(catalogElement) %}
<a href="/{{ st_imageconverter(mainImage, 'image_zoom') }}" class="border">
    <img src="/{{ st_imageconverter(mainImage, 'thumb_big') }}" alt="" class="cloudzoom" id="zoom_1" data-cloudzoom="zoomSizeMode: 'image', lazyLoadZoom: true" />
</a>

{# get the image list #}
{% set imageList = ses_assets_image_list(catalogElement) %}
{% for img in imageList %}
    <img src="/{{ st_imageconverter(img, 'thumb_smaller') }}" alt="" />
{% endfor %}

{# get some other assets, like pdfs #}
{% set manuals = ses_assets_by_group(catalogElement, 'Manuals') %} 
{% for manual in manuals %}
    <a href="{{ asset(manual.path) }}">{{ manual.fileName }}</a>
{% endfor %}
```

## Product variants

If you want to get the assets for a specific variant of a catalog element, you have to set the optional parameter `$productId`.

``` html+twig
{# create the productId e.g. 1110_green #}
{% set productId = catalogElement.sku ~ '_green' %}

{# get the main image #}
{% set mainImage = ses_assets_main_image(catalogElement, productId) %}
<a href="/{{ st_imageconverter(mainImage, 'image_zoom') }}" class="border">
    <img src="/{{ st_imageconverter(mainImage, 'thumb_big') }}" alt="" class="cloudzoom" id="zoom_1" data-cloudzoom="zoomSizeMode: 'image', lazyLoadZoom: true" />
</a>
```

## Configuration

!!! note "separator"

    The `separator` key is not required, but you can specify it if you want to use acustom separator.
    By default `_` is used.

??? note "EshopBundle/Resources/config/silver.eshop.yml"

    ``` yaml
    parameters:
        silver_eshop.default.catalog_factory.assets:

            # possible values: true, false - switch the assets to on/off
            active: true

            # possible values: merge, dataprovider, storage
            strategy: merge

            Silversolutions\Bundle\EshopBundle\Product\CatalogNode:
                Images:
                    folder: "var/assets/product_images"
                    types: [ "jpg", "png" ]
                    subFolder: 2
                    target: "image"
                Downloads:
                    folder: "var/assets/product_downloads"
                    types: [ "exe", "zip" ]
                    subFolder: 2
                    target: "attachment"
                    separator: " "
               Manuals:
                    folder: "var/assets/manuals"
                    types: "pdf"
                    subFolder: 2
                    target: "attachment"
                    separator: "_"
             CustomPDFs:
                    folder: "var/assets/pdfs"
                    types: "pdf"
                    subFolder: 2
                    target: "attachment"

            Silversolutions\Bundle\EshopBundle\Product\OrderableProductNode:
                Images:
                    folder: "var/assets/product_group_images"
                    types: [ "jpg", "png" ]
                    subFolder: 2
                    target: "image"
                Downloads:
                    folder: "var/assets/product_group_downloads"
                    types: [ "exe", "zip" ]
                    subFolder: 2
                    target: "attachment"
                Manuals:
                    folder: "var/assets/manuals"
                    types: "pdf"
                    subFolder: 2
                    target: "attachment"
                CustomPDFs:
                    folder: "var/assets/pdfs"
                    types: "pdf"
                    subFolder: 2
                    target: "attachment"

            Silversolutions\Bundle\EshopBundle\Product\VariantProductNode:
                Images:
                    folder: "var/assets/product_group_images"
                    types: [ "jpg", "png" ]
                    subFolder: 2
                    target: "image"
                Downloads:
                    folder: "var/assets/product_group_downloads"
                    types: [ "exe", "zip" ]
                    subFolder: 2
                    target: "attachment"
                Manuals:
                    folder: "var/assets/manuals"
                    types: "pdf"
                    subFolder: 2
                    target: "attachment"
                CustomPDFs:
                    folder: "var/assets/pdfs"
                    types: "pdf"
                    subFolder: 2
                    target: "attachment"
    ```

### Webserver configuration

Webserver configuration needs a new rewrite rule (see [Configuration](../../guide/configuration/configuration.md#http-server-configuration)).

## Cache

Assets can now be cached using Stash. Cache is created depending both on `productId` and group settings.

### Configuration:

|Parameter|Description|
|--- |--- |
|`silver_eshop.default.asset_cache`|When set to true, translation cache is enabled|
|`silver_eshop.default.asset_cache_ttl`|Defines how long the item is stored in the cache. When TTL value is set to null, cache is stored forever.|

``` yaml
silver_eshop.default.asset_cache: true
# possible values: null, integer
silver_eshop.default.asset_cache_ttl: null
```
