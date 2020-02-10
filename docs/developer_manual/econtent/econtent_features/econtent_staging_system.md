#  econtent - Staging system 

econtent supports a staging area. The product catalogue can exists as a 

  - live version which is used for customer
  - a staging version (tmp) which can be used for staging and import purposes

eZ Commerce offers tools to switch between the productive and staged version. 

## Best practice 

Depending on your import strategy eZ Commerce offers different options:

  - Full import to the tmp tables without touching the productive system  
    This strategy support large product catalogs as well. After the import process the result of the import can be checked (e.g. number of products compared between the live and temp tables) and depending on the results the new catalog can be put live.
  - A delta update which directly uses the productive system
  - The staging system can be used for emergency situations as well:  
    If a product catalogue causes serious issues (legacy issues, wrong data or structure) an SQL dump from past imports can be imported without Interruption of the production system.

## How to deal which live and staging (tmp) areas

Create the index for the new products (tmp area). This command indexes the data from the tmp tables to a tmp core.

``` 
php bin/console silversolutions:indexecontent --siteaccess=import
```

**Important**: Please us a siteaccess (default: import) for the indexing process. The siteaccess import should cover:

  - all languages used in econtent (see ezplatform.yml)
  - by default the table set for this siteaccess is configured to use the tmp tables

The product import can e.g. use the tmp tables. The import process will not stop the productive system. This still uses the productive version. 

## How to switch between tmp and live 

After the the import the DB tables and solr cores have to be switched:

``` 
php bin/console silversolutions:indexecontent swap
php bin/console silversolutions:econtent-tables-swap
```

Depending on the project the httpcache for the product catalog should be purged.
