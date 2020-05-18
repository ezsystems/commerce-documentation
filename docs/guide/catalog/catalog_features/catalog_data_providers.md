# Catalog data providers

eZ Commerce provides a flexible way to handle products and catalogs. The shop uses central data providers to fetch data for the shop (concrete implementations of `CatalogDataProvider`).

![](../../img/catalog_dataproviders.png)

The products are dynamically injected into the Content Tree regardless of which source is used to provide the products.

## Catalog data from the content model

Catalog data from the content model is provided using `Ez5CatalogDataProvider`.

This option can be used when you have no PIM system and the amount of products is limited.
It can be used when the catalog contains up to 20.000 products

Products can be edited directly in the Back Office.

Importing products when using this data provider is time-consuming.

## Catalog data from eContent

Catalog data from eContent is provided using `EcontentCatalogDataProvider`.

This option is used when you use a PIM system or an ERP which provides all product information.
This information can be imported quickly from the relevant system.

The catalog can then contain more than 1 million products.

Products cannot be edited in the Back Office.
