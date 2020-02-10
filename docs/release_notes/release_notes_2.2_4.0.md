#  Release notes 2.2 (4.0) 

**Not an official version\!**

**Must not be visible to the public.**

## eZ Commerce version 2.2

The version 2.2 mainly supports the eZ Platform Version V2.2. containing the new Page Builder 

  - supported eZ Version V.2.2  

 If you apply this eZ Commerce version in the project, always check the latest tagged version.

## Server requirements

 eZ Commerce relies on eZ Platform software that is built to rely on existing technologies and standards, mainly:

  - PHP scripting language: 7.1  

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
<p>#16478</p>
</td>
<td><a href="https://doc.silver-eshop.de/pages/viewpage.action?pageId=2654503">P</a>ageBuilder </td>
<td><p>New Page Builder has been adapted to eZ Commerce</p>
<p>A new search component has been introduced for the Page Builder block "product slider"</p>
<p>New Templates and configuration for zones and embed templates for products, events and blog posts</p></td>
</tr>
<tr>
<td>#16382</td>
<td>Richtext field</td>
<td><p>Custom Tag product slider</p>
<p>Allows to embed a product slider inside a richtext offering 2 designs</p></td>
</tr>
<tr>
<td>#16287</td>
<td>Product</td>
<td>eZ Commerce offers a possibility to upload and offer a pdf download for products</td>
</tr>
<tr>
<td>#15825</td>
<td>installer</td>
<td>Installer for eZ Commerce</td>
</tr>
</tbody>
</table>

## Bug fixes

List of bugs that were fixed in this version.

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
#16410<br />

</td>
<td>Registration</td>
<td>Registration form broken - privacy policies checkbox<br />
</td>
</tr>
<tr>
<td>#16376</td>
<td>configuration</td>
<td><p>Issue with configuration settings</p>
<p>The option RemotePrice provider has been offered in eZ Commerce as well which is not correct</p></td>
</tr>
</tbody>
</table>

# API Changes

## eZ Commerce

### Database changes

#### Update doctrine

``` 
php bin/console doctrine:schema:update --force
```

### Configuration

no change

### Classes

no change

### Method signature changes

no change

  -  

### Interface changes

no change

### Deprecated content

no change

### Ez legacy changes and files/settings ignored by .git 

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

## silver.orderhistory

Follow these changes only, if you are using [Orderhistory](#) in your project.

no change

## silver.customercenter

Follow these changes only, if you are using [Customercenter](#) in your project.

no change

## Hotfixes

To check the API changes for the hotfixes, please check the hotfix patches above\!
