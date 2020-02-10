#  SesExternalData 

Advanced version only

This datatype `sesexternaldatatype` use external storage (not directly eZ) to store data. Data **MUST** be stored in following table.

### Table `ses_externaldata`:

<table>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Field
</th>
<th><div class="tablesorter-header-inner">
Type
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>sku</code></td>
<td><code>char(40)</code></td>
<td>Unique ID of the <a href="23560458.html">Product category (CatalogElement)</a></td>
</tr>
<tr>
<td><code>identifier</code></td>
<td><code>char(40)</code></td>
<td><div class="content-wrapper">
<p>the ID of the field</p>

<p>Definition</p>
<div class="aui-message hint shadowed information-macro">
<span class="aui-icon icon-hint"> 

<p>constant prefix (ses) + lower case letters from NAV fields</p>
<p>Example:</p>
<pre><code>VENDOR_NO  --&gt; ses_vendor_no</code></pre>
</td>
</tr>
<tr>
<td><code>language_code</code></td>
<td><code>char(8)</code></td>
<td>e.g. <code>ger-DE</code></td>
</tr>
<tr>
<td><code>ses_field_type</code></td>
<td><p><code>char(20)</code></p></td>
<td><ul>
<li>The datatype used for this data.</li>
<li>It is possible to use silver-e.shop FieldType:<br />

<ul>
<li><code>ArrayField</code></li>
<li><code>FileField</code></li>
<li><code>ImageField</code></li>
<li><code>PriceField</code></li>
<li><code>StockField</code></li>
<li><code>TextBlockField</code></li>
<li><code>TextLineField</code></li>
</ul></li>
<li>or just a simple datatype to store data<br />

<ul>
<li><code>int</code></li>
<li><code>float</code></li>
<li><code>bool</code></li>
</ul></li>
</ul></td>
</tr>
<tr>
<td><code>content</code></td>
<td><code>longtext</code></td>
<td>serialized data in string format.</td>
</tr>
</tbody>
</table>

### Create ses\_externaldata table

**Create database table**

``` 
CREATE TABLE `ses_externaldata` (
  `sku` char(40) NOT NULL,
  `identifier` char(40) NOT NULL,
  `language_code` char(8) NOT NULL,
  `ses_field_type` char(20) NOT NULL,
  `content` longtext,
  PRIMARY KEY (`sku`,`identifier`,`language_code`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

# How to store data in `ses_externaldata` table

Data that is stored in the ses\_externaldata table must be either a simple datatype: *int, float, boolean* or a [FieldType](Fields-for-ecommerce-data_23560470.html).

### Data format

#### silver-e.shop FieldTypes

The data content will be stored in the database in *serialized form* using the method **toHash()**.

``` 
//Product with sku: 308488, NAV-Field: WERBETEXT_KURZ, content: 'Die Enten mit Kultcharakter. Als Geschenk immer wieder begehrt. Machen Sie Ihren Kunden die Freude.', ses_field_type: TextBlockField
 
$blockField = new TextBlockField(
        array(
            'text' => 'Die Enten mit Kultcharakter. Als Geschenk immer wieder begehrt. Machen Sie Ihren Kunden die Freude.',
        )
    );
$content = serialize($blockField->toHash());
 
$dbStatement = "INSERT INTO ses_externaldata VALUES(308488, ses_werbetext_kurz, ger-DE, TextBlockField, $content)";
```

#### Simple datatypes

Simpe datatypes (int, float, bool) with be stored in *serialized form*.

``` 
//Product with sku: 308488, NAV-Field: GEWICHT, content: 60, ses_field_type: int
 
$content = serialize(60);
 
$dbStatement = "INSERT INTO ses_externaldata VALUES(308488, ses_gewicht, ger-DE, int, $content)";
```

# Symfony Datatype

Symfony Datatype is stored in:

    Silversolutions/Bundle/DatatypesBundle/FieldType/SesExternalData/*

    Silversolutions/Bundle/DatatypesBundle/Converter/SesExternalData.php

The member attribute $text of FieldType\\SesExternalData\\Value is actually an array. Do not assign strings to this (public) attribute as all implementations rely on the PHP type array. An example of how the structure of the array looks like, can be seen later in this document.

#### Configuration

**Silversolutions/Bundle/DatatypesBundle/Resources/config/services.xml**

``` 
    <parameters>
       <parameter key="ezpublish.fieldType.sesexternaldata.class">Silversolutions\Bundle\DatatypesBundle\FieldType\SesExternalData\Type</parameter>
       <parameter key="ezpublish.fieldType.sesexternaldata.converter.class">Silversolutions\Bundle\DatatypesBundle\Converter\SesExternalData</parameter>    
    </parameters>

    <services>      
        <!-- sesexternaldata type service -->
        <service id="ezpublish.fieldType.sesexternaldata" class="%ezpublish.fieldType.sesexternaldata.class%" parent="ezpublish.fieldType">
            <tag name="ezpublish.fieldType" alias="sesexternaldata" />
        </service>

        <!-- sesexternaldata converter service -->
        <service id="ezpublish.fieldType.sesexternaldata.converter" class="%ezpublish.fieldType.sesexternaldata.converter.class%">
            <argument type="service" id="ezpublish.api.storage_engine.legacy.dbhandler" />
            <argument type="service" id="ezpublish.config.resolver" />
            <argument type="service" id="silver_common.logger" />
            <tag name="ezpublish.storageEngine.legacy.converter" alias="sesexternaldata"  />
        </service> 
    </services> 
```

# Handling of sesexternaldata in eZ DataProvider

#### Adding new field in the product class

It is possible to extend the product class by the sesexternaldata type.

![](attachments/23560305/23563975.png)

#### Connecting to the external storage

If the table ses\_externaldata is filled properly with the data, it is possible to create the connection in eZ with appropriate product by typing the SKU.

![](attachments/23560305/23563981.png)

![](attachments/23560305/23563978.png)

``` 
 
```

#### Handling the fetched data

The data from sesexternaldata is converted in the shop and an array of [FieldTypes](Fields-for-ecommerce-data_23560470.html) or simple types is returned

![](attachments/23560305/23563814.png)

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-04-10 um 14.41.47.png](attachments/23560305/23563975.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-04-10 um 14.44.16.png](attachments/23560305/23563981.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-04-10 um 14.44.25.png](attachments/23560305/23563978.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-04-10 um 14.51.28.png](attachments/23560305/23563814.png) (image/png)  
