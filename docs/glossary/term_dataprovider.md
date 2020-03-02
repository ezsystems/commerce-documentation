# Term - Dataprovider

**Dataprovider** is the instance that provides the catalog data. eZ Commerce (Advanced version only) provides a flexible way to handle products and categories. Catalog data can be stored in different source in the shop (eZ, DB, solr) and the main goal of the data provider is to fetch the product data from the source and build the catalog elements.

Currently there are two concrete implementations for dataproviders:

||||
|--- |--- |--- |
|Catalog Data CMS eZ</br>(Ez5CatalogDataProvider)|products can be edited in the CMS</br>good for up to 20.000 products</br>NO import of products costs time|customer has no PIM system</br>and the amount of products|
|Catalog Data eContent</br>(EcontentCatalogDataProvider)|allows more than 1 million products</br>fast imports</br>currently NO edit interface in the backend of the CMS|The customer is using a PIM system</br>or the ERP provides all the product information|
