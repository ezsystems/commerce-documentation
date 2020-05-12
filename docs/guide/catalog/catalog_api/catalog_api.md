# Catalog API

## Accessing product data

When the URL loads a product, a `CatalogElement` object is provided to the template. 

``` html+twig
<!-- name of the product -->
<h1 class="u-ellipsis">{{ catalogElement.name }}</h1>
 
<strong>{{ 'SKU: %sku%'|st_translate(null, {'%sku%': catalogElement.sku}) }}</strong>

<p>{{ ses_render_field(catalogElement, 'shortDescription') }}</p>
<!-- show a video link -->
<a class="video_link" href="{{ ses_render_field(catalogElement, 'video') }}">
       <span class="sprite sprite-032b-video-30 trans">
</a>
 
<!-- Display an image -->
 <figure class="text-center u-no-margin u-padding-1x u-border">
            {% if orderableVariantProduct|default is not empty %}
                {% set productId = catalogElement.sku ~ orderableVariantProduct.variantCode %}
                {% set mainImage = ses_assets_main_image(orderableVariantProduct, productId)  %}
            {% endif %}
            {% if mainImage is empty %}
                {% set mainImage = ses_assets_main_image(catalogElement)  %}
            {% endif %}
            <a href="/{{ st_imageconverter(mainImage, 'image_zoom') }}">
                <img src="/{{ st_imageconverter(mainImage, 'thumb_big') }}" alt="" class="cloudzoom" 
                 id="zoom_1" data-cloudzoom="zoomSizeMode: 'image', lazyLoadZoom: true, autoInside: 640" />
            </a>
            <figcaption class="text-left u-ellipsis">{{ catalogElement.name }}</figcaption>
 </figure>
```

In a controller or server you can easily access product data:

``` php
use Silversolutions\Bundle\EshopBundle\Services\Catalog;
 
     
$skuValue = "5512";
 
// @var CatalogService $catalogService
$catalogService = $this->getContainer()->get('silver_catalog.data_provider_service');
 
$dataProvider = $catalogService->getDataProvider();

$product =  $dataProvider->fetchElementBySku($skuValue);
 
if ($product instanceof OrderableProductNode) {
    $productName = $product->name;
    $unitPrice = $product->price->price->priceInclVat;
}
```

Fetch a `catalogElement` by Location ID:

``` php
/** @var $catalogService \Silversolutions\Bundle\EshopBundle\Services\Catalog\CatalogDataProviderService */
$catalogService = $this->get('silver_catalog.data_provider_service');
try {
    /** @var EzHelperService $ezHelper */
    $ezHelper = $this->get('silver_tools.ez_helper');
    /** @var $catalogElement \Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement */
    $catalogElement = $catalogService->getDataProvider()->fetchElementByIdentifier(
        200,
        $ezHelper->getCurrentLanguageCode()
    );
 } catch (\Exception $e) {
    return $this->render(
        'SilversolutionsEshopBundle:Catalog:exception.html.twig',
        array(
            'exception' => $e
        ),
        $response
    );
}
```

### List of CatalogElements

The `fetchChildrenList()` method enables listing `CatalogElement` or `ProductNode` objects for a given Location ID.
Products with this Location ID are returned.

Fetch products directly assigned to a `CatalogElement` starting form the first element (offset=0, limit=100).
The shop uses the language of the current SiteAccess.

``` php
$catalogList = $catalogService->getDataProvider()
        ->fetchChildrenList($locationId, 1, array(), null, 0, 100);
```

Fetch a list of products using a filter named `productList`.

``` 
$catalogList = $catalogService->getDataProvider()
            ->fetchChildrenList($locationId, 1, array('filterType' => 'productList'), null, 0, 100);
```

## Filtering

For each data provider filters can be defined. A filter defines:

- which elements are fetched (e.g. just products such as `ses_product`, or just product groups)
- sorting

A filter's name (e.g. `navigation`) is used as a key to get the filter parameters from the config file.

For a list of standard filters see [Catalog configuration](../catalog_configuration.md).
