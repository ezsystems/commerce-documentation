# Operation Catalog

This class implements business logic for basket

#### **Methods**

|method|parameters|returns|purpose|operation identifier|
|--- |--- |--- |--- |--- |
|loadProducts|InputLoadList $input|OutputLoadList $input|load products from catalog|catalog.load_products|


#### Service definition

services.business_layer.xml:

``` xml
<parameters>
    <parameter key="ses_eshop.catalog.business_api.class">Silversolutions\Bundle\EshopBundle\Services\BusinessLayer\Operations\Catalog</parameter>
</parameters>

<!-- catalog business component -->
<service id="ses_eshop.catalog.business_api" class="%ses_eshop.catalog.business_api.class%"
             parent="ses_eshop.business_api.base">
        <argument type="service" id="silver_catalog.catalog_service" />
        <tag name="business_api_operation" alias="catalog" />
</service>
```
