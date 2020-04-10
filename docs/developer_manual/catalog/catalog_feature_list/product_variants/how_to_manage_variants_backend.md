# How to manage variants (backend)

## When are Product Variants created?

This depends on the dataprovider used. eZ Commerce offers a standard way if the source eZ Platform is used: 

If the ProductNode contains information in the matrix (**ses\_variants**), the Ez5CatalogFactory creates [VariantProductNode](23560374.html) instead of OrderableProductNode.

Because the [VariantProductNode](23560374.html) is not orderable, a Service- [VariantService](Variant-Services_23560238.html) is used to create an OrderableVariantNode when **add to basket** is called.

## How to setup variants in eZ Platform

In order to provide variants the product class `ses_product` has to be extended with a new attribute with identifier `ses_variants` using the datatype uivarvarianttype. 

By default eZ Commerce offers a set of variant types. The variant type defines which attributes such as size or/and color are used to identify a variant.

The variant types can be defined using a yml configuration. Technical details are described here:  [VariantType](VariantType_23560699.html)

### Example form for variants using 2 levels (size and color) in the backend of the CMS

![](../../../img/product_variants.png)
