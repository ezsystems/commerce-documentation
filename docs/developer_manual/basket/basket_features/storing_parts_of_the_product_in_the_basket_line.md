# Storing parts of the product in the basket line

This service is used for the serializing and unserializing of catalog element. This service implements the **SerializeServiceInterface**.

This feature ensures that the basket always contains infos about the product which should be ordered. In case the customer is sending an order the shop can ensure sure that all important product data for the order is present even if the product was meanwhile removed from the product catalog. 

The parameter "refreshCatalogElementAfter" defined how long the catalogelement will be used from the basket line until it will be refreshed from the catalog.

``` 
ses_basket.default.refreshCatalogElementAfter: 1 hours
```

The service is using all attributes of the catalog element, that are set in the configuration:

**basket.yml**

``` yaml
    #defines catalog element attributes that should be stored in a basket line
    siso_basket.default.stored_catalog_element_attributes:
        Silversolutions\Bundle\EshopBundle\Product\OrderableProductNode:
            baseAttributes: 
                - customerPrice
                - ean
                - identifier
                - image
                - imageList
                - isOrderable
                - longDescription
                - manufacturerSku
                - maxOrderQuantity
                - minOrderQuantity
                - allowedQuantity
                - name
                - packagingUnit
                - parentElementIdentifier
                - path
                - price
                - scaledPrices
                - shortDescription
                - sku
                - specifications
                - stock
                - text
                - type
                - unit
                - url
                - vatCode
                - displayInSearch
                - displayInProductList
                - section
            dataMapAttributes: []
        Silversolutions\Bundle\EshopBundle\Product\OrderableVariantNode:
            baseAttributes: 
               - customerPrice
               - ean
               - identifier
               - image
               - imageList
               - isOrderable
               - longDescription
               - manufacturerSku
               - maxOrderQuantity
               - minOrderQuantity
               - allowedQuantity
               - name
               - packagingUnit
               - parentElementIdentifier
               - path
               - price
               - scaledPrices
               - shortDescription
               - sku
               - specifications
               - stock
               - text
               - type
               - unit
               - url
               - vatCode
               - variantCode
               - variantCharacteristics
               - displayInSearch
               - displayInProductList
               - section
            dataMapAttributes: [] 
```

!!! note

    By default, all base attributes are set in the configuration and the dataMap attributes are empty.

    You can override this configuration, but pay attention, that you have specified at least all required base attributes, otherwise it is not possible to create a new CatalogElement from the serialized version.

!!! tip

    Also make sure, that the attributes, that you have specified are either simple datatypes (int, float, boolean, string, array) or instances of [FieldInterface](Fields-for-ecommerce-data_23560470.html).

    All other objects will be ignored, because it is not possible to assure that the serializing process will work properly.

## CatalogElementSerializationListener

To store the CatalogElement in the basket line the doctrine listener is used. This listener makes an usage of the [serialize service](Storing-parts-of-the-product-in-the-basket-line_23560572.html).

!!! note
    
    Namespace:

    `Silversolutions\Bundle\EshopBundle\EventListener\Basket`

    ID:

    `siso_basket.catalog_element_serialization_listener`

This listener listens to different events to make sure that the catalog element is ALWAYS from type CatalogElement in the shop but longtext (serialized string) in the database.

``` php
   /**
     * Returns an array of events this subscriber wants to listen to.
     *
     * @return array
     */
    public function getSubscribedEvents()
    {
        return array(
            Events::postLoad,
            Events::prePersist,
            Events::preUpdate,
            Events::postUpdate,
            Events::postPersist,
        );
    }
```

Depending on the event the catalog element from basket line is either serialized to a string or unserialized to a CatalogElement.
