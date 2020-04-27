# eContent

## What is eContent?

eContent is a storage provider for eZ Commerce which can store product data in a very efficient way. It allows to store data (mostly for products and product groups) in a database tables with a simple structure. 

During the import the database tables are filled and the eContent data provider and eContent factory are there to get the information from the database and create catalog elements.

Main advantages:

- Fast imports (e.g. from ERP or PIM systems)
- Supports more than one million products
- Search ready
- Fast access
- Avoids to store products in the CMS content model
- Allows imports during production and switching catalogs

## Data provider eZ vs. eContent

eZ Commerce uses an almost generic way to access the catalogue. The catalogue can be stored in eZ or in eContent. 

Nevertheless if eContent is used there are a few restrictions which have to be considered:

- A product cannot be embeded using a standard eZ platform embed feature. eZ Commerce offers an alternative feature which allows to embed products in richttext fields using a data provider independent way
- eContent products will not be visible in the backend

It depends on the requirements of the customer to decide which provider should be used. The following table compares the main features:

| Feature                              | eZ                                           | econtent                                     |
| ------------------------------------ | -------------------------------------------- | -------------------------------------------- |
| Flexible datamodel                   | yes  | yes  |
| Translations                         | yes  | yes  |
| Interface for editing                | yes  | no |
| Versioning                           | yes  | no |
| Simple segmentation feature          | yes  | yes  |
| Extended segmentation feature        | no | yes  |
| Fast imports (e.g. from PIM)         | no | yes  |
| Supports large catalogs              | no | yes  |
| Staging feature (live and tmp space) | no | yes  |

## Best way to start

econtent has a lot of features. If you want to get into the topic as fast as possible please check these documents:

- Check the [econtent - Features](econtent_features/econtent_features.md) section to learn which features are supported
- Learn how econtent is organizing the data in the DB - [Econtent dataprovider - database model](econtent_features/econtent_dataprovider_database_model.md)
- Learn howto import data into econtent - Using the API - [How to import products (API)](econtent_cookbook/econtent_how_to_import_products/how_to_import_products_api.md)

Please check important settings described in [econtent - Configuration](econtent_configuration.md) (especially in section "Important configuration for projects")
