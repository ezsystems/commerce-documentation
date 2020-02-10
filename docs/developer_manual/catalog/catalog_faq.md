# Catalog - FAQ 

How can i change the standard image-not-available.png image?

 Define the path to the image as a parameter:

``` 
parameters:
 silver_tools.default.defaultImage: ../web/bundles/<yourprojectbundle>/img/image-not-available.png
```

Create a new image in your Bundle:

src/\<project\>/Bundle/\<BundleName\>/Resources/public/img/image-not-available.png

Please remove the already generated image first:

``` 
rm web/var/ecommerce/storage/images/thumb_medium/i/m/a/-image-not-available_thumb_medium.png
```

Are the prices setup in the backend of eZ including VAT?

This can be configured by siteaccess:

``` 
parameters:
    siso_core.default.standard_price_factory.is_vat_price: true
```

By default it is set to true so prices provided in the backend of the CMS are including VAT.
