#  Catalog - API 

## How to access product data

When a product is loaded by the url a CatalogElement object is provided to the template. 

``` 
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

In a Controller or Server you can access product data easily:

``` 
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

Fetch a catalogElement by locationId

``` 
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

### List of catalogElements

This method allows to list CatalogElements or ProductNodes for a given locationid. Products assigned to a locationid will be returned.

Fetch products directly assigned to a CatalogElement starting form the first element (offset=0, limit=100). The shop shall use the current language of the siteaccess used. 

``` 
    $catalogList = $catalogService->getDataProvider()
            ->fetchChildrenList($locationId, 1, array(), null, 0, 100);
```

Fetch a list of products using a filter named "productList".

``` 
 $catalogList = $catalogService->getDataProvider()
            ->fetchChildrenList($locationId, 1, array('filterType' => 'productList'), null, 0, 100);
```

## Filtering

Foreach dataprovider filters can be defined. A filter defines 

  - which elements shall be fetched (e.g. just products such as ses\_product, just product groups)
  - sorting

A filter has a name. The name will be used as a key to get the filter parameters from the config file. 

A filter name is e.g. "navigation". 

Standard filters see [Catalog - Configuration](Catalog---Configuration_23560211.html)
