# Release notes 2.5 (4.1.1)

## eZ Commerce version 2.5 (4.1.1)

- intermediate version
- supported eZ Platform v2.5  

If you apply this eZ Commerce version in the project, always check the latest tagged version.

## Server requirements

eZ Commerce relies on eZ Platform software that is built to rely on existing technologies and standards, mainly:

- PHP scripting language: 7  
- SQL database: *MySql/MariaDB or PostgreSQL*
- Web Server: Apache/Nginx
- Java JRE  (Oracle-Sun/OpenJDK) when Solr is used (for use with eZ Find search engine)

## New features and improvements

List of new features and improvements.

|Ticket|Topic|Description|
|--- |--- |--- |
|#16227|Logging|By default it is logged, if a token was processed. For example when a user activates his account by double Opt-in.|
|#17140|Database|New Index for ses_basket table|
|#17427|Contact form|Prefill form with customer data|
|#17468|Specifications Field Type|Extend Field Type with unit field|
|#17281|Bestseller|Add bestseller for econtent|
|#17421|Customer Profile Data|New edit form for Customer Profile Data in backend|
|#17317|eZ Design Engine|eZ Design Engine can be used for templating|
|#17596|ERP|New transportlayer for the ERP Microsoft Dynamic NAV for the Webconnector of TSO data|

## Bug fixes

List of bugs that were fixed in this version.

|Ticket|Topic|Description|
|--- |--- |--- |
|#17176|Ordermanagement|Fix URL generation of return parameter for lost order process|
|#16349|Search|Set Proper filter parameters for product list|
|#17370|Facets|If only one facet value exists the facet was not visible. But the open/close facets links were visible. With this bugfix the links will not be displayed in this use case.|
|#17166|Cockpit Econtent|Count of products in cockpit was corrected for econtent as dataprovider|
|#17456|Configuration settings|Storing configuration settings in file system removed.|
|#17785|Checkout delivery address|Customer without customer number can choose existing delivery addresses|
|#17341|Create order in ERP for customer without customer number|For ERP connection templatedebitorservice is used to get ERP template customer number for customer without own customer number.|
|#17592|Command|Replace wrong command option 'lc' with 'l'|
|#17495|Orderhistory|Fix NotFoundException for siteaccess with deactivated ERP|
|#17828|Orderhistory|Line sums did not show discount|
|#17784|Edit variants in basket|Remove duplicated stock icon|
|#17321|Vouchers|Fix issue where vouchers are not merged correctly with their baskets|
|#17426|Customer Profile Data|FetchProfileData method of EzErpCustomerProfileDataService was overwriting dataMap, if no customer number was set.|
|#17326|Price toggler|Hide/show prices on all pages instead of one page only.|
|#17425|Customer Profile Data|Erp Request was sent with deactivated ERP connection.|
|#17387|autosuggest|Fix for missing image|
|#17825|Landingpage editor|Fixed productslider styling issue|
|#17541|Twig Extension|Replaced usage of wrong RichTextExtension with richTextConverter|
|#17878|Page builder|Broken zones removed|
|#17879|Backend|Erroneous folder template fixed|
|#17889|Backend|Unnecessary service alias removed. It has caused broken image previews|
|#17586|Backend|Fix console errors while viewing products|
|#17887|Backend|Fix console errors while viewing users|
|#17908|Backend|Missing translations for field types in Content Type view added|
|#17912|Page Builder|Fix issue where 'is_visibility_enabled does not exist' error was thrown when opening Product Slider settings|
|#17921|Form Builder|Add missing labels for radio and checkbox fields in form view|
|#17923|Backend|Fixed issue where schedular tab wasn't loading|

## JavaScript libraries and plugins

no change

# API Changes

## eZ Commerce

### Database changes

#### Update doctrine

``` bash
php ezpublish/console doctrine:schema:update --force
```

### Configuration

no change

### Classes

no changes

### Services

### Method signature changes

no change

### Interface changes

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

## Hotfixes

To check the API changes for the hotfixes, please check the hotfix patches above!
