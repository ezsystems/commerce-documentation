# Landing Page API

## Last viewed products

The products are recorded using a JavaScript call. In order to use it on other pages as well you have to place this code in your template:

`{{ render_hinclude( controller( ``"SilversolutionsEshopBundle:Catalog:storeProductAsLastViewed"``, {``"sku"``: catalogElement.sku} ) ) }}`

After each call the cache for the last viewed product slider is purged.
By default the view is rendered by an ESI block and cached per user.
When the customer visits a new product the list is purged and regenerated the next time it is displayed. 

Products are stored in the session.

### Displaying products

``` html+twig
{{ render_esi(
   controller(
      'SilversolutionsEshopBundle:EzFlow:showLastViewedProducts',
       { }
   ))
}}
```

The controller is able to render a different template if required (parameter template).
`SilversolutionsEshopBundle:Catalog:slider.html.twig` is used by default. 

The caching strategy can be defined in the config file. Since it is dynamic, you should use `vary: cookie`.

``` yaml
silver_eshop.default.http_cache:
        last_viewed_products:
            max_age: 36000
            vary: cookie
```

You can also configure the number of stored products (10 by default):

``` yaml
silver_eshop.default.last_viewed_products_in_session_limit: 10
```

## Bestsellers

The Bestseller block is rendered using content ID (if a product category is selected)
or a category ID (if the input field is used).

If the selected Content item is not a category, an error message will be displayed. 

Make sure that the Bestseller feature is enabled:

``` 
siso_core.default.enable_bestsellers: true
```
