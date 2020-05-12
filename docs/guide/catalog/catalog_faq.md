# Catalog FAQ

## How can I change the standard image-not-available.png image?

Define the path to the image as a parameter:

``` yaml
parameters:
    silver_tools.default.defaultImage: ../web/bundles/<yourprojectbundle>/img/image-not-available.png
```

Create a new image in your bundle:

`src/<project>/Bundle/<BundleName>/Resources/public/img/image-not-available.png`

Remove the already generated image first:

``` bash
rm web/var/ecommerce/storage/images/thumb_medium/i/m/a/-image-not-available_thumb_medium.png
```

## Do the prices set up in the Back Office include VAT?

This can be configured by siteaccess:

``` yaml
parameters:
    siso_core.default.standard_price_factory.is_vat_price: true
```

By default it is set to true so prices provided in the Back Office include VAT.
