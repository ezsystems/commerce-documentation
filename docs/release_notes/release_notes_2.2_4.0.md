# Release notes 2.2 (4.0)

**Not an official version!**

## eZ Commerce version 2.2

The version 2.2 mainly supports the eZ Platform Version v2.2 containing the new Page Builder 

- supported eZ Platform v2.2  

If you apply this eZ Commerce version in the project, always check the latest tagged version.

## Server requirements

eZ Commerce relies on eZ Platform software that is built to rely on existing technologies and standards, mainly:

- PHP scripting language: 7.1  
- SQL database: *MySql/MariaDB or PostgreSQL*
- Web Server: Apache/Nginx
- Java JRE  (Oracle-Sun/OpenJDK) when Solr is used (for use with eZ Find search engine)

## New features and improvements

List of new features and improvements.

|Ticket|Topic|Description|
|--- |--- |--- |
|#16478|PageBuilder|New Page Builder has been adapted to eZ Commerce</br>A new search component has been introduced for the Page Builder block "product slider"</br>New Templates and configuration for zones and embed templates for products, events and blog posts|
|#16382|Richtext field|Custom Tag product slider</br>
Allows to embed a product slider inside a richtext offering 2 designs|
|#16287|Product|eZ Commerce offers a possibility to upload and offer a pdf download for products|
|#15825|installer|Installer for eZ Commerce|

## Bug fixes

List of bugs that were fixed in this version.

|Ticket|Topic|Description|
|--- |--- |--- |
|#16410|Registration|Registration form broken - privacy policies checkbox|
|#16376|configuration|Issue with configuration settings</br>The option RemotePrice provider has been offered in eZ Commerce as well which is not correct|

# API Changes

## eZ Commerce

### Database changes

#### Update doctrine

``` bash
php bin/console doctrine:schema:update --force
```

### Configuration

no change

### Classes

no change

### Method signature changes

no change

### Interface changes

no change

### Deprecated content

no change

### eZ Legacy changes and files/settings ignored by .git 

no change  

### Templates

no change

### JS changes

no change

### Vendor bundles + config

no change

### Overridden vendor services

no change  

## Translations

### Textmodules

no change

## Topic-specific (not complex) changes in eZ Commerce

no change  

## silver.orderhistory

Follow these changes only, if you are using Order history in your project.

no change

## silver.customercenter

Follow these changes only, if you are using Customer Center] in your project.

no change

## Hotfixes

To check the API changes for the hotfixes, please check the hotfix patches above!
