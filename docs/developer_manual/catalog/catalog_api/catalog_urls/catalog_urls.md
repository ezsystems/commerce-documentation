# Catalog urls

The URLs for products and catalog will be generate by a central URL service system.

## Usage

When a URL is requested in a twig template, as described in [Catalog - UI](Catalog---UI_23560463.html), the [URL service](Catalog-urls_23560474.html) will be called in the background.

URLs will be provided by special methods enclosed in catalogElement:

- SEO URL `{{ catalogElement.seoUrl }}`
- Permantent URL `{{ catalogElement.permanentUrl }}`

Accessing the seoUrl or permanentUrl method of a catalogElement will trigger the generation of the appropriate URL.

``` 
$seoUrlOfAProduct = $aProductNode.getSeoUrl();
```

## Configuration

The service uses a configuration file for specify the logic how to generate SEO URL and permanent URLs. 

The configuration file `silver.eshop.yml` provides two settings:

`silver.e-shop.yml`:

``` yaml
parameters:
    silver_eshop.default.path_prefix_seo: /silver.catalog
    silver_eshop.default.path_prefix_permanent: /silver.catalog/Products
```

The parameter `silver_eshop.default.path_prefix_seo` provides a prefix for all catalog URLs. In this case the URLs for the catalog will always start with the prefix `/silver.catalog`.

In addition to this the routing may have to be changed if the prefix changes:

`routing.yml`:

``` yaml
silversolutionsEshopBundle:
    pattern: /silver.catalog/{url}
    defaults:
        _controller: SilversolutionsEshopBundle:Catalog:show
        url: %silver_eshop.default.url_silver_catalog%
    requirements:
        url: ".+"
```

Placeholder for {url} parameter is set in

`silver.eshop.yml`:

``` yaml
parameters:
    silver_eshop.default.url_silver_catalog: Products
```
