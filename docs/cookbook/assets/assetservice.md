#  AssetService 

It is possible that images or other files (like pdf, exe, zip...) are not stored in CatalogElement (**dataprovider**), but stored in a special folder on the web-server (**storage**). The file names contain sku (later with variant code) for assigning the files to the product.

There is a special AssetService, that checks the given strategy and gets all assets for a single CatalogElement.

AssetServices uses the [StorageService](StorageService_23560715.html) to get the assets from the storage.

### Strategy

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>merge</th>
<th>dataprovider</th>
<th>storage</th>
</tr>
</thead>
<tbody>
<tr>
<td><ul>
<li>mainImage from the dataprovider is used</li>
<li>imageList from dataprovider is used - if catalogElement contains imageList - and all images from storage are stored in imageList</li>
</ul></td>
<td><ul>
<li>mainImage from the dataprovider is used</li>
<li>imageList from dataprovider is used - if catalogElement contains imageList</li>
</ul></td>
<td><ul>
<li>mainImage from the dataprovider is overwritten by storage</li>
<li>imageList from storage is used</li>
</ul></td>
</tr>
</tbody>
</table>

## AssetService

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><br />
</th>
<th>AssetService</th>
</tr>
</thead>
<tbody>
<tr>
<td>Id</td>
<td>silver_catalog.asset_service</td>
</tr>
<tr>
<td>Meaning</td>
<td><p>AssetService represents the entry point to the CatalogElement assets. It</p>
<p>determines the proper strategy, how the assets are stored in CatalogElement.</p>
<p>It may call the StorageService, which gets the assets from the file system and</p>
<p>stores all assets for a single CatalogElement.</p></td>
</tr>
</tbody>
</table>

##### **AssetService Functions**

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Parameters</th>
<th>Returns</th>
<th>Meaning</th>
<th>TWIG Helper function</th>
</tr>
</thead>
<tbody>
<tr>
<td>getAssetsByGroup()</td>
<td><p>CatalogElement $catalogElement</p>
<p>string $settingsGroup</p>
<p>string $productId = null</p></td>
<td>array()</td>
<td><div class="content-wrapper">
<p>returns all assets for a single CatalogElement by settingGroup</p>

$productId is optional, if it isn´t set, by default the sku of catalogElement is used.
</td>
<td><p>{{ <strong>ses_assets_by_group</strong>(catalogElement, 'Manuals', catalogElement.sku) }}</p>
<p>{{ ses_assets_by_group(catalogElement, 'Manuals') }}</p></td>
</tr>
<tr>
<td>getMainImage()</td>
<td><p>CatalogElement $catalogElement</p>
<p>string $productId = null</p></td>
<td>FileField or null</td>
<td><div class="content-wrapper">
<p>returns the main Image for the catalogElement</p>

$productId is optional, if it isn´t set, by default the sku of catalogElement is used.
</td>
<td><p>{{ <strong>ses_assets_main_image</strong>(catalogElement, catalogElement.sku) }}</p>
<p>{{ ses_assets_main_image(catalogElement) }}</p></td>
</tr>
<tr>
<td>getImageList()</td>
<td><p>CatalogElement $catalogElement</p>
<p>string $productId = null</p></td>
<td>array of FileFields or null</td>
<td><div class="content-wrapper">
<p>returns a list of images for the catalogElement</p>

$productId is optional, if it isn´t set, by default the sku of catalogElement is used.
</td>
<td><p>{{ <strong>ses_assets_image_list</strong>(catalogElement, catalogElement.sku) }}</p>
<p>{{ ses_assets_image_list(catalogElement) }}</p></td>
</tr>
</tbody>
</table>

##### Usage:

``` 
//in the controller
$assetService = $this->get('silver_catalog.asset_service');
$mainImage = $assetService->getMainImage($catalogElement);
```

##### Usage inside the twig template:

``` 
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

If you want to get the assets for a specific variant of the catalogElement, you have to set the optional parameter $productId

``` 
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

separator

Please notice that the '**separator**' is not required, but you can specify it, if you want to use custom one. By default '\_' is used.

**\~EshopBundle/Resources/config/silver.eshop.yml** Expand source 

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

Please be aware the webserver configuration need a new rewrite rule (see [Configuration](Configuration_23560547.html)).

## Cache

Assets can now be cached using Stash. Cache is created depending both on productId and group settings.

#### Configuration:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>silver_eshop.default.asset_cache</code></pre></td>
<td>When set to true then translation cache is enabled</td>
</tr>
<tr>
<td><pre><code>silver_eshop.default.asset_cache_ttl</code></pre></td>
<td><div class="content-wrapper">
<p>Defines how long the Item will be stored in the cache.</p>

<p>When TTL value is set to null, then cache is generated forever.</p>
</td>
</tr>
</tbody>
</table>

**silver.eshop.yml**

``` 
silver_eshop.default.asset_cache: true
# possible values: null, integer
silver_eshop.default.asset_cache_ttl: null
```
