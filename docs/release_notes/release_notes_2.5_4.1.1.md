#  Release notes 2.5 (4.1.1) 

## eZ Commerce version 2.5 (4.1.1)

  - intermediate version
  - supported eZ Platform Version V2.5  

 If you apply this eZ Commerce version in the project, always check the latest tagged version.

## Server requirements

eZ Commerce relies on eZ Platform software that is built to rely on existing technologies and standards, mainly:

  - PHP scripting language: 7  

  - SQL database: *MySql/MariaDB or PostgreSQL*

  - Web Server: Apache/Nginx

  - Java JRE  (Oracle-Sun/OpenJDK) when Solr is used (for use with eZ Find search engine)

## New features and improvements

List of new features and improvements.

<table>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<p><a href="http://rm.extranet.silversolutions.de/issues/16227" class="external-link">#16227</a><br />
</p>
</td>
<td>Logging</td>
<td>By default it is logged, if a token was processed. For example when a user activates his account by double Opt-in.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17140" class="external-link">#17140</a></td>
<td>Database</td>
<td>New Index for ses_basket table</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17427" class="external-link">#17427</a></td>
<td>Contact form</td>
<td>Prefill form with customer data</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17468" class="external-link">#17468</a></td>
<td>Specifications Field Type</td>
<td>Extend Field Type with unit field</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17281" class="external-link">#17281</a></td>
<td>Bestseller</td>
<td>Add bestseller for econtent</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17421" class="external-link">#17421</a></td>
<td>Customer Profile Data</td>
<td>New edit form for Customer Profile Data in backend</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17317" class="external-link">#17317</a></td>
<td>eZ Design Engine</td>
<td>eZ Design Engine can be used for templating</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17596" class="external-link">#17596</a></td>
<td>ERP</td>
<td>New transportlayer for the ERP Microsoft Dynamic NAV for the Webconnector of TSO data</td>
</tr>
</tbody>
</table>

## Bug fixes

List of bugs that were fixed in this version.

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 15%" />
<col style="width: 69%" />
</colgroup>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<a href="http://rm.extranet.silversolutions.de/issues/17176" class="external-link">#17176</a><br />

</td>
<td>Ordermanagement</td>
<td>Fix URL generation of return parameter for lost order process<br />
</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16349" class="external-link">#16349</a></td>
<td>Search</td>
<td><p>Set Proper filter parameters for product list</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17370" class="external-link">#17370</a></td>
<td>Facets</td>
<td>If only one facet value exists the facet was not visible. But the open/close facets links were visible. With this bugfix the links will not be displayed in this use case.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17166" class="external-link">#17166</a></td>
<td>Cockpit Econtent</td>
<td>Count of products in cockpit was corrected for econtent as dataprovider</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17456" class="external-link">#17456</a></td>
<td>Configuration settings</td>
<td>Storing configuration settings in file system removed.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17785" class="external-link">#17785</a></td>
<td>Checkout delivery address</td>
<td>Customer without customer number can choose existing delivery addresses</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17341" class="external-link">#17341</a></td>
<td>Create order in ERP for customer without customer number</td>
<td>For ERP connection templatedebitorservice is used to get ERP template customer number for customer without own customer number.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17592" class="external-link">#17592</a></td>
<td><p>Command</p></td>
<td>Replace wrong command option 'lc' with 'l'</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17495" class="external-link">#17495</a></td>
<td>Orderhistory</td>
<td>Fix NotFoundException for siteaccess with deactivated ERP </td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17828" class="external-link">#17828</a></td>
<td>Orderhistory</td>
<td>Line sums did not show discount</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17784" class="external-link">#17784</a></td>
<td>Edit variants in basket</td>
<td>Remove duplicated stock icon</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17321" class="external-link">#17321</a></td>
<td>Vouchers</td>
<td>Fix issue where vouchers are not merged correctly with their baskets</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17426" class="external-link">#17426</a></td>
<td><p>Customer Profile Data</p></td>
<td><p>FetchProfileData method of EzErpCustomerProfileDataService was overwriting dataMap, if no customer number was set.</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17326" class="external-link">#17326</a></td>
<td>Price toggler</td>
<td>Hide/show prices on all pages instead of one page only.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17425" class="external-link">#17425</a></td>
<td>Customer Profile Data</td>
<td>Erp Request was sent with deactivated ERP connection.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17387" class="external-link">#17387</a></td>
<td><p>autosuggest</p></td>
<td>Fix for missing image</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17825" class="external-link">#17825</a></td>
<td><p>Landingpage editor</p></td>
<td>Fixed productslider styling issue</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17541" class="external-link">#17541</a></td>
<td>Twig Extension</td>
<td><p>Replaced usage of wrong RichTextExtension with richTextConverter</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17878" class="external-link">#17878</a></td>
<td>Page builder</td>
<td>Broken zones removed</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17879" class="external-link">#17879</a></td>
<td>Backend</td>
<td><p>Erroneous folder template fixed</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17889" class="external-link">#17889</a></td>
<td>Backend</td>
<td>Unnecessary service alias removed. It has caused broken image previews</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17586" class="external-link">#17586</a></td>
<td>Backend</td>
<td>Fix console errors while viewing products</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17887" class="external-link">#17887</a></td>
<td>Backend</td>
<td>Fix console errors while viewing users</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17908" class="external-link">#17908</a></td>
<td>Backend</td>
<td><p>Missing translations for field types in Content Type view added</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17912" class="external-link">#17912</a></td>
<td>Page Builder</td>
<td>Fix issue where 'is_visibility_enabled does not exist' error was thrown when opening Product Slider settings</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17921" class="external-link">#17921</a></td>
<td>Form Builder</td>
<td>Add missing labels for radio and checkbox fields in form view</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17923" class="external-link">#17923</a></td>
<td>Backend</td>
<td>Fixed issue where schedular tab wasn't loading</td>
</tr>
</tbody>
</table>

## JavaScript libraries and plugins

no change

# API Changes

## eZ Commerce

### Database changes

#### Update doctrine

``` 
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

## Topic specific (not complex) changes in eZ Commerce 

no change

## Hotfixes

To check the API changes for the hotfixes, please check the hotfix patches above\!
