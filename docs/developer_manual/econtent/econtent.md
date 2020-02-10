#  eContent 

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

  - A product cannot be embeded using a standard eZ platform embed feature. eZ Commerce offers an alternative feature which allows to embed products in richttext fields using a data provider independend way
  - eContent products will not be visible in the backend

It depends on the requirements of the customer to decide which provider should be used. The following table compares the main features:

| Feature                              | eZ                                           | econtent                                     |
| ------------------------------------ | -------------------------------------------- | -------------------------------------------- |
| Flexible datamodel                   | ![(tick)](images/icons/emoticons/check.png)  | ![(tick)](images/icons/emoticons/check.png)  |
| Translations                         | ![(tick)](images/icons/emoticons/check.png)  | ![(tick)](images/icons/emoticons/check.png)  |
| Interface for editing                | ![(tick)](images/icons/emoticons/check.png)  | ![(error)](images/icons/emoticons/error.png) |
| Versioning                           | ![(tick)](images/icons/emoticons/check.png)  | ![(error)](images/icons/emoticons/error.png) |
| Simple segmentation feature          | ![(tick)](images/icons/emoticons/check.png)  | ![(tick)](images/icons/emoticons/check.png)  |
| Extended segmentation feature        | ![(error)](images/icons/emoticons/error.png) | ![(tick)](images/icons/emoticons/check.png)  |
| Fast imports (e.g. from PIM)         | ![(error)](images/icons/emoticons/error.png) | ![(tick)](images/icons/emoticons/check.png)  |
| Supports large catalogs              | ![(error)](images/icons/emoticons/error.png) | ![(tick)](images/icons/emoticons/check.png)  |
| Staging feature (live and tmp space) | ![(error)](images/icons/emoticons/error.png) | ![(tick)](images/icons/emoticons/check.png)  |

## Best way to start

econtent has a lot of features. If you want to get into the topic as fast as possible please check these documents:

  - Check the [econtent - Features](econtent---Features_23561034.html) section to learn which features are supported
  - Learn how econtent is organizing the data in the DB -  [Econtent dataprovider - database model](Econtent-dataprovider---database-model_23560823.html)
  - Learn howto import data into econtent - Using the API - [How to import products (API)](23561078.html)

Please check important settings described in [econtent - Configuration](econtent---Configuration_23561029.html) (especially in section "Important configuration for projects")
