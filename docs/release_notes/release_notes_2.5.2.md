#  Release notes 2.5.2 (4.1.3) 
ses-brand version 2.5.1 (4.1.2)

  - intermediate version
  - supported eZ Version V2.5  
 If you apply this %ses-brand version in the project, always check the latest tagged version.

## Server requirements

%ses-brand relies on eZ Publish software that is built to rely on existing technologies and standards, mainly:

  - PHP scripting language: 7  
  - SQL database: *MySql/MariaDB or PostgreSQL*

  - Web Server: Apache/Nginx

  - Java JRE  (Oracle-Sun/OpenJDK) when Solr is used (for use with eZ Find search engine)

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
<tr class="odd">
<td><div class="content-wrapper">
<p><br />
</p>
</td>
<td><br />
</td>
<td><br />
</td>
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
<tr class="odd">
<td><div class="content-wrapper">
<a href="http://rm.extranet.silversolutions.de/issues/18627" class="external-link">#18627</a>
</td>
<td><p>EzUserHelper</p></td>
<td><p>Deprecated/Removed criterion still used in EzUserHelper</p></td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18650" class="external-link">#18650</a></td>
<td>AuthenticationProvider</td>
<td><p>AuthenticationProvider not using correct Interface</p></td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18733" class="external-link">#18733</a></td>
<td><p>OlarkService</p></td>
<td>Wrong typehint used</td>
</tr>
<tr class="even">
<td><div class="content-wrapper">
<p><a href="http://rm.extranet.silversolutions.de/issues/18212" class="external-link">#18212</a><br />
</p>
</td>
<td>Wrong VAT calculation</td>
<td><span style="color: rgb(0,0,0);">Fix wrong VAT calculation in shoppriceengine</td>
</tr>
</tbody>
</table>

## New plugins

Plugins that were implemented for this version.

| Ticket                                            | Name                                                            | Description                                                                     |
| ------------------------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| \#12345 | [TestPluginBundle](#) | This plugin offers test functionality |

## Tagged versions

List of tagged versions. These tags are created in the next development phase - 2.5.2++. This list must be fulfilled when the next release was published.

### silver.eShop

<table>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
<th>Date</th>
<th>Tag</th>
<th>Patch</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
#12345<br />

</td>
<td>Example</td>
<td>fixed a bug</td>
<td>23-02-15</td>
<td>3.1-1234</td>
<td><div class="content-wrapper">
<p><a href="#" class="unresolved">EZP-27011-solr-index-location-change.patch</a><br />
</p>
</td>
</tr>
</tbody>
</table>

### silver.customercenter

<table>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
<th>Date</th>
<th>Tag</th>
<th>Patch</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
#12345<br />

</td>
<td>Example</td>
<td>fixed a bug</td>
<td>23-02-15</td>
<td>3.1-1234</td>
<td><div class="content-wrapper">
<div class="content-wrapper">
<p><a href="#" class="unresolved">EZP-27011-solr-index-location-change.patch</a><br />
</p>

</td>
</tr>
</tbody>
</table>

### silver.orderhistory

<table>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
<th>Date</th>
<th>Tag</th>
<th>Patch</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
#12345<br />

</td>
<td>Example</td>
<td>fixed a bug</td>
<td>23-02-15</td>
<td>3.1-1234</td>
<td><div class="content-wrapper">
<div class="content-wrapper">
<p><a href="#" class="unresolved">EZP-27011-solr-index-location-change.patch</a><br />
</p>

</td>
</tr>
</tbody>
</table>

## JavaScript libraries and plugins

NameVersion jQuery1.7.2
# API Changes

## silver.eShop

### Database changes

#### Update doctrine

```
php ezpublish/console doctrine:schema:update --force
```

  - document custom queries that need to be executed manually (e.g. migrate old data structure to the new schema)
  - document manual changes in tables

### Configuration

  - document modified configuration settings in e-shop
  - document all parameters, that are ignored by git (parameters.yml), but need to be added/modified in the project

### Classes

  - ***move/rename the class***
      - document every change in the namespace - document the **old** file name and the **new** file name

<!-- end list -->

  - ***constants***
      - document every change in the constants (constant name or value) - document the **old** constant name and the **new** constant name
  - ***changed attributes***
      - document the class name
    
      - document **old** and **new** attribute name (in the project the old name must be replaced with new one - in every place)
    
      - add some examples, how to change it:
        
        **TODO - TO BE REMOVED**
        
        
        ```
        - @property-read float $vatCode
        + @property-read float $vatPercen
        ```
        
        **TODO: TO BE REMOVED**
        
        
        ```
        # example how to use it
        return new PriceField(
            array(
                'price' => $this->priceFactoryService->createPrice(
                    array(
                        'price' => (float)$dataMap[$fieldIdentifier]->text,
                        'isVatPrice' => true,
                        'source' => 'eZ',
        -                'vatCode' => 19.00
        +                'vatPercent' => 19.00
                    )
                )
            )
        );
        ```
        
        
        
        
  - ***new attributes***
      - document, that in the project there is a need to check, if the model was overriden and if so, new attribute must be added
      - document the class name
      - document the new attribute name
      - document all places where the attribute must be added (e.g. configuration)

### Services

  - document any constructor changes
  - document configuration changes for this service
  - document changes in service id

### Method signature changes

  - document all signature changes

  - add examples
**TODO: TO BE REMOVED**

```
- protected function extractPrice($fieldIdentifier, $dataMap)
+ protected function extractPrice($fieldIdentifier, $dataMap, $vatCode = null, $vatPercent = null)
```

### Interface changes

  - document every change of add/remove/update of method in the interface

### Deprecated content

  - document deprecated methods/attributes/classes...
  - document alternative to these deprecated content  
    
  - document how to use the alternative instead of the old code (maybe link to the existing documentation, if it exists)  
    
  - or document if there is no alternative and the deprecated content will not be supported anymore\!

### Templates

  - document paths to removed templates
  - document paths to changed templates
  - diff for the changes templates will be created on the end  
### JS changes

  - document all .js changes

### Vendor bundles + config

If silver.eShop requires some vendor bundles (solr, paypal...), document how to:

  - install them
  - enable them
  - configure them

### Overridden vendor services

If silver.eShop overrides some standard services (from eZ or Symfony), this has to be documented\! 

## Translations

### Textmodules

  - Document all new textmodules  
## Topic specific (not complex) changes in silver.eShop 

  - document all changes that belong to one topic here
## silver.orderhistory

Follow these changes only, if you are using [Orderhistory](#) in your project.

  - Check the API changes structure from above
## silver.customercenter
Follow these changes only, if you are using [Customercenter](#) in your project.

  - Check the API changes structure from above

## Hotfixes

To check the API changes for the hotfixes, please check the hotfix patches above\!
