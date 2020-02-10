#  Econtent dataprovider - database model 

![econtent\_structure.gif](http://confluence.extranet.silversolutions.de:8090/download/attachments/10617018/econtent_structure.gif?version=1&modificationDate=1396614956000&api=v2)

econtent is using 4 main database tables and 2 optional ones:

<table>
<thead>
<tr class="header">
<th><pre><code>table</code></pre></th>
<th>Staging</th>
<th><pre><code>purpose</code></pre></th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>sve_class</code></pre></td>
<td><br />
</td>
<td><pre><code>Stores types of catalog elements (ex. product, product_group). Every type has an identifier, which has a relation with sve_class_attributes</code></pre></td>
</tr>
<tr>
<td><pre><code>sve_class_attributes</code></pre></td>
<td><br />
</td>
<td><pre><code>Stores all possible catalog element attributes (name, type) for different catalog elements (class_id)</code></pre></td>
</tr>
<tr>
<td><pre><code>sve_object</code></pre></td>
<td><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /></td>
<td><pre><code>Stores all catalog elements and general information about them (url, parent, depth etc)</code></pre></td>
</tr>
<tr>
<td><pre><code>sve_object_attributes</code></pre></td>
<td><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /></td>
<td><pre><code>Stores all attributes for the given catalog element depending on language.</code></pre></td>
</tr>
<tr>
<td><pre><code>ses_externaldata</code></pre></td>
<td><br />
</td>
<td><pre><code>optional: Stores additional information for sve_object_attributes of &quot;ses_externaldata&quot;. It collect more information for that kind of catalog element.</code></pre></td>
</tr>
<tr>
<td>sve_object_catalog</td>
<td><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /></td>
<td><pre><code>optional: used for segmentation</code></pre></td>
</tr>
</tbody>
</table>

For staging purposes the DB tables are using a prefix "\_tmp"  (e.g. sve\_object\_tmp). The staging tables can be used to import a complete product catalogue without affecting the production catalogue. Details see [econtent - Staging system](econtent---Staging-system_23561079.html). 

eZ Commerce Advanced makes usage of Doctrine entities in order to create these tables\!

Pay attention, that it is not possible to create indexes for the table `sve_object_attributes` for the `data_text` attribute via Doctrine\! Therefore you would have to create the indexes manually every time you run

`php bin/console doctrine:schema:update --force`

## sve\_object - metadata for products and product groups

<div class="level3">

The table *sve\_object* contains exactly one entry for each product group, product, etc. It is possible to arrange data in a tree structure using the field 'parent\_id', which is the node-ID of the parent. Node-IDs are beginning at a value of 2 due to reasons of compatibility to the data structure of eZ Platform.

This table contains several other pieces of information in addition to class-ID and node-ID, for example :

  - 
    
    <div class="li">
    
    time of last change

  - 
    
    <div class="li">
    
    node-ID of the parent

  - 
    
    <div class="li">
    
    flag whether this object is blocked

  - 
    
    <div class="li">
    
    priority

  - 
    
    <div class="li">
    
    url alias - readable URL of this document (for example '/shop/spielwaren/kinder/holz\_spielzeug')

  - 
    
    <div class="li">
    
    depth

  - 
    
    <div class="li">
    
    main node-ID - if this object is related somewhere else, this defines the first appearance in this tree, where all data referenced to this object is stored

### Table sve\_object

<div class="level2">

<div class="table sectionedit4">

<table>
<thead>
<tr class="header">
<th>Field</th>
<th>Type</th>
<th>Null</th>
<th>Key</th>
<th>Default</th>
<th>Extra</th>
</tr>
</thead>
<tbody>
<tr>
<td>node_id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td>PRI</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>class_id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td>MUL</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>parent_id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td><br />
</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>change_date</td>
<td>datetime</td>
<td>YES</td>
<td><br />
</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>blocked</td>
<td>tinyint(3) unsigned</td>
<td><br />
</td>
<td><br />
</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>priority</td>
<td>smallint(5) unsigned</td>
<td><br />
</td>
<td><br />
</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>section</td>
<td>tinyint(3) unsigned</td>
<td><br />
</td>
<td><br />
</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>url_alias</td>
<td>text</td>
<td>YES</td>
<td>MUL</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>path_string</td>
<td>varchar(255)</td>
<td>YES</td>
<td>MUL</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>depth</td>
<td>tinyint(3) unsigned</td>
<td>YES</td>
<td><br />
</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>main_node_id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td>MUL</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>hidden</td>
<td>tinyint(4)</td>
<td>YES</td>
<td><br />
</td>
<td>0</td>
<td><br />
</td>
</tr>
</tbody>
</table>

Example:

``` code
mysql > select node_id, class_id, parent_id, blocked, hidden, priority from sve_object limit 1,3;
+---------+----------+-----------+---------+--------+----------+
| node_id | class_id | parent_id | blocked | hidden | priority |
+---------+----------+-----------+---------+--------+----------+
|       3 |        1 |         2 |       0 |      0 |        7 | 
|       4 |        1 |         3 |       0 |      0 |        1 | 
|       5 |        2 |         4 |       0 |      0 |        0 | 
+---------+----------+-----------+---------+--------+----------+

select node_id, url_alias, path_string, depth from sve_object limit 1,3;
+---------+--------------------------------------------------------------------+-------------+-------+
| node_id | url_alias                                                          | path_string | depth |
+---------+--------------------------------------------------------------------+-------------+-------+
|       3 | Produkte/Plakate                                                   | /2/3/       |     2 | 
|       4 | Produkte/Plakate/Motivplakate                                      | /2/3/4/     |     3 | 
|       5 | Produkte/Plakate/Motivplakate/Bildung-macht-Zukunft                | /2/3/4/5/   |     4 | 
+---------+--------------------------------------------------------------------+-------------+-------+
```

## sve\_object\_attributes - attributes for products and product groups

<div class="level3">

Corresponding to the class definition (see sve\_class and sve\_class\_attributes), you can create multiple data fields for each entry in *sve\_object\_attributes*. Each of them can be set in its own language. The language codes are specified in ISO-639 (for example 'ger-DE' or 'eng-US'). 

Each entry consists of the following fields:

  - 
    
    <div class="li">
    
    attribute\_id - ID of this attribute (see sve\_class\_attributes)

  - 
    
    <div class="li">
    
    node\_id - internal node-ID

  - 
    
    <div class="li">
    
    data\_int, data\_float, data\_text - value of the attribute depending on the data type (see sve\_class\_attributes)

### Table sve\_object\_attributes

<div class="level2">

<div class="table sectionedit7">

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Field</th>
<th>Type</th>
<th>Null</th>
<th>Key</th>
<th>Default</th>
<th>Extra</th>
</tr>
</thead>
<tbody>
<tr>
<td>node_id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td>PRI</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>attribute_id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td>PRI</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>data_float</td>
<td>float</td>
<td>YES</td>
<td><br />
</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>data_int</td>
<td>int(11)</td>
<td>YES</td>
<td><br />
</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>data_text</td>
<td>text</td>
<td>YES</td>
<td><br />
</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>language</td>
<td>varchar(6)</td>
<td><br />
</td>
<td>PRI</td>
<td>ger-DE</td>
<td><br />
</td>
</tr>
</tbody>
</table>

### Table sve\_class

<div class="level3">

This table describes all available classes. The class fields are defined in sve\_class\_attributes.

<div class="table sectionedit9">

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Field</th>
<th>Type</th>
<th>Null</th>
<th>Key</th>
<th>Default</th>
<th>Extra</th>
</tr>
</thead>
<tbody>
<tr>
<td>class_id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td>PRI</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>class_name</td>
<td>varchar(255)</td>
<td>YES</td>
<td><br />
</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>name_identifier</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td><br />
</td>
<td>0</td>
<td><br />
</td>
</tr>
</tbody>
</table>

### Tabelle sve\_class\_attributes

<div class="level3">

The table describes the class fields.

<div class="table sectionedit11">

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Field</th>
<th>Type</th>
<th>Null</th>
<th>Key</th>
<th>Default</th>
<th>Extra</th>
</tr>
</thead>
<tbody>
<tr>
<td>attribute_id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td>PRI</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>class_id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td><br />
</td>
<td>0</td>
<td><br />
</td>
</tr>
<tr>
<td>attribute_name</td>
<td>varchar(255)</td>
<td>YES</td>
<td><br />
</td>
<td>NULL</td>
<td><br />
</td>
</tr>
<tr>
<td>ezdatatype</td>
<td>varchar(255)</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>sort_field</td>
<td>varchar(255)</td>
<td><br />
</td>
<td><br />
</td>
<td>data_text</td>
<td><br />
</td>
</tr>
</tbody>
</table>

The following datatypes are supported:

#### ezstring

<div class="level4">

The data is stored in the field data\_text as a flat string.

#### ezinteger

<div class="level4">

The data is stored in the field data\_int.

#### ezprice

<div class="level4">

The price information is stored in the field data\_float. The field data\_text contains information about the VAT value in percent and a flag indicating if the price is incluing VAT or not.

Example:

``` code
+---------+--------------+------------+----------+-----------+----------+
| node_id | attribute_id | data_float | data_int | data_text | language |
+---------+--------------+------------+----------+-----------+----------+
|       5 |          210 |      0.952 |        0 | 19,1      | ger-DE   | 
+---------+--------------+------------+----------+-----------+----------+
```

  - 
    
    <div class="li">
    
    The price is 0.952 EUR (currency is defined per shop)

  - 
    
    <div class="li">
    
    The VAT is 19% and the price is including VAT).

#### ezmatrix (deprecated)

<div class="level4">

This datatype allows to store data organized in rows in columns in one field.

This is useful for dynamic attributes. The data is stored in a defined XML format.

**Example:**

``` 
+---------+--------------+------------+----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------+
| node_id | attribute_id | data_float | data_int | data_text                                                                                                                                                                                                      | language |
+---------+--------------+------------+----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------+
|     249 |          240 |          0 |        0 | <?xml version='1.0' encoding='UTF-8'?><ezmatrix><name></name><columns number='2'><column num='0' id='filename'>Filename</column><column num='1' id='name'>Name</column></columns><rows number='0'/></ezmatrix> | ger-DE   | 
+---------+--------------+------------+----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------+
```

  - 
    
    <div class="li">
    
    columns: definition of the columns using a id and a Label

  - 
    
    <div class="li">
    
    rows: containing the data by rows and columns. if the table contains 2 rows a 2 columns 4 \<c\> Tags have to be generated\!

``` 
<?xml version='1.0' encoding='UTF-8'?>
<ezmatrix>
    <name></name>
    <columns number='2'>
        <column num='0' id='filename'>Filename</column>
        <column num='1' id='name'>Name</column>
    </columns>
    <rows number='1'/>
    <c>test/30ccf286d5c8cf1a3ebb16e75cb0adc4.pdf</c>
    <c>GS60-Montageanleitung.pdf</c>
</ezmatrix> 
```

<div class="level4">

## Examples

``` 
# Classes defined for econtent
 
mysql > SELECT * FROM sve_class;
class_id    class_name      name_identifier
1           warengruppe     101 
2           bestellprodukt  201
 
# attributes for the classes 1 and 2
 
mysql > SELECT * FROM sve_class_attributes;
attribute_id    class_id    attribute_name  ezdatatype  sort_field
101             1           name            ezstring    data_text   
102             1           navisionid      ezstring    data_text
201             2           name            ezstring    data_text
202             2           navisionid      ezstring    data_text
203             2           description     ezstring    data_text
204             2           vendor_no       ezstring    data_text
205             2           unit_list_price ezstring    data_text
 
 
# object table 
mysql > SELECT * FROM sve_object; 
node_id class_id    parent_id   change...   blocked priority    section url_alias   path... depth   main...
2   1           0           NULL    0   1           0   shop            /2/ 1   2
3   1           2           NULL    0   1           0   shop/wg         /2/3    2   3
4   2           3           NULL    0   1           0   shop/wg/bsp /2/3/4  3   4
 
 
# attributes
 
SELECT * FROM sve_object_attributes;
node_id attribute_id    data_float  data_int    data_text       language
2   101         NULL            NULL            Shop            ger-DE
2   102         NULL            NULL            FOLDER_ID       ger-DE
3   101         NULL            NULL            Wg              ger-DE
3   102         NULL            NULL            FOLDER_ID       ger-DE
4   201         NULL            NULL            Bsp             ger-DE
4   202         NULL            NULL            ITEM_NO         ger-DE
4   203         NULL            NULL            Beschreibung    ger-DE
4   204         NULL            NULL            019-DA-12       ger-DE
4   205         NULL            NULL            19.95           ger-DE
```

<div class="level3">

### Table ses\_externaldata

<div class="level3">

This table describes all external data from (SAP, PIM, TYP etc.). The content is encoded in json.  The class fields are defined in sve\_class\_attributes.

<div class="table sectionedit9">

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Field</th>
<th>Type</th>
<th>Null</th>
<th>Key</th>
<th>Default</th>
<th>Extra</th>
</tr>
</thead>
<tbody>
<tr>
<td>id</td>
<td>int(10) unsigned</td>
<td><br />
</td>
<td>PRI</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>sku</td>
<td>varchar(40)</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><p>field_id</p></td>
<td>varchar(40)</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>language_code</td>
<td>varchar(8)</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>ses_field_type</td>
<td>varchar(20)</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>content</td>
<td>longtext</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td>json encoded</td>
</tr>
</tbody>
</table>

Matching external data from "sve\_class\_attributes" of type "**ses\_externaldata**" is done by data\_text. It must match the "sku" (ex. 000000000000167738)
