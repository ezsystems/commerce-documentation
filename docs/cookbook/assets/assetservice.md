# AssetService

It is possible that images or other files (like pdf, exe, zip...) are not stored in CatalogElement (**dataprovider**), but stored in a special folder on the web-server (**storage**). The file names contain sku (later with variant code) for assigning the files to the product.

There is a special AssetService, that checks the given strategy and gets all assets for a single CatalogElement.

AssetServices uses the [StorageService](storageservice.md) to get the assets from the storage.

### Strategy

|merge|dataprovider|storage|
|--- |--- |--- |
|mainImage from the dataprovider is used</br>imageList from dataprovider is used - if catalogElement contains imageList - and all images from storage are stored in imageList|mainImage from the dataprovider is used</br>imageList from dataprovider is used - if catalogElement contains imageList|mainImage from the dataprovider is overwritten by storage</br>imageList from storage is used|

## AssetService

||AssetService|
|--- |--- |
|Id|silver_catalog.asset_service|
|Meaning|AssetService represents the entry point to the CatalogElement assets. It determines the proper strategy, how the assets are stored in CatalogElement. It may call the StorageService, which gets the assets from the file system and stores all assets for a single CatalogElement.|


##### **AssetService Functions**

|Function|Parameters|Returns|Meaning|TWIG Helper function|
|--- |--- |--- |--- |--- |
|getAssetsByGroup()|CatalogElement $catalogElement</br>string $settingsGroup</br>string $productId = null|array()|returns all assets for a single CatalogElement by settingGroup</br>$productId is optional, if it isn´t set, by default the sku of catalogElement is used.|{{ses_assets_by_group(catalogElement, 'Manuals', catalogElement.sku) }}</br>{{ ses_assets_by_group(catalogElement, 'Manuals') }}|
|getMainImage()|CatalogElement $catalogElement</br>string $productId = null|FileField or null|returns the main Image for the catalogElement</br>$productId is optional, if it isn´t set, by default the sku of catalogElement is used.|{{ ses_assets_main_image(catalogElement, catalogElement.sku) }}</br>{{ ses_assets_main_image(catalogElement) }}|
|getImageList()|CatalogElement $catalogElement</br>string $productId = null|array of FileFields or null|returns a list of images for the catalogElement</br>$productId is optional, if it isn´t set, by default the sku of catalogElement is used.|{{ ses_assets_image_list(catalogElement, catalogElement.sku) }}</br>{{ ses_assets_image_list(catalogElement) }}|

##### Usage:

``` php
//in the controller
$assetService = $this->get('silver_catalog.asset_service');
$mainImage = $assetService->getMainImage($catalogElement);
```

##### Usage inside the twig template:

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

## Product VARIANTs

If you want to get the assets for a specific variant of the catalogElement, you have to set the optional parameter `$productId`.

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

The assets configuration is stored in **silver.eshop.yml**

!!! note "separator"

    Please notice that the '**separator**' is not required, but you can specify it, if you want to use custom one. By default '\_' is used.

??? note "~EshopBundle/Resources/config/silver.eshop.yml"

    ``` 
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

Please be aware the webserver configuration need a new rewrite rule (see [Configuration](../../enhanced_configuration/configuration/configuration.md)).

## Cache

Assets can now be cached using Stash. Cache is created depending both on productId and group settings.

#### Configuration:

|Parameter|Description|
|--- |--- |
|silver_eshop.default.asset_cache|When set to true then translation cache is enabled|
|silver_eshop.default.asset_cache_ttl|Defines how long the Item will be stored in the cache.</br>When TTL value is set to null, then cache is generated forever.|

**silver.eshop.yml**

``` yaml
silver_eshop.default.asset_cache: true
# possible values: null, integer
silver_eshop.default.asset_cache_ttl: null
```
