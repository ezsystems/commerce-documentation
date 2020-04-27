# Econtent dataprovider - database model

econtent is using 4 main database tables and 2 optional ones:

|table|Staging|purpose|
|--- |--- |--- |
|sve_class||Stores types of catalog elements (ex. product, product_group). Every type has an identifier, which has a relation with sve_class_attributes|
|sve_class_attributes||Stores all possible catalog element attributes (name, type) for different catalog elements (class_id)|
|sve_object|yes|Stores all catalog elements and general information about them (url, parent, depth etc)|
|sve_object_attributes|yes|Stores all attributes for the given catalog element depending on language.|
|ses_externaldata||optional: Stores additional information for sve_object_attributes of "ses_externaldata". It collect more information for that kind of catalog element.|
|sve_object_catalog|yes|optional: used for segmentation|


For staging purposes the DB tables are using a prefix `_tmp`  (e.g. `sve_object_tmp`). The staging tables can be used to import a complete product catalogue without affecting the production catalogue. Details see [econtent - Staging system](econtent_staging_system.md). 

!!! note

    eZ Commerce Advanced makes usage of Doctrine entities in order to create these tables!

    Pay attention, that it is not possible to create indexes for the table `sve_object_attributes` for the `data_text` attribute via Doctrine\! Therefore you would have to create the indexes manually every time you run

    `php bin/console doctrine:schema:update --force`

## sve_object - metadata for products and product groups

The table *sve_object* contains exactly one entry for each product group, product, etc. It is possible to arrange data in a tree structure using the field 'parent_id', which is the node-ID of the parent. Node-IDs are beginning at a value of 2 due to reasons of compatibility to the data structure of eZ Platform.

This table contains several other pieces of information in addition to class-ID and node-ID, for example :

- time of last change
- node-ID of the parent
- flag whether this object is blocked
- priority
- url alias - readable URL of this document (for example '/shop/spielwaren/kinder/holz_spielzeug')
- depth
- main node-ID - if this object is related somewhere else, this defines the first appearance in this tree, where all data referenced to this object is stored

### Table sve_object

|Field|Type|Null|Key|Default|Extra|
|--- |--- |--- |--- |--- |--- |
|node_id|int(10) unsigned||PRI|0||
|class_id|int(10) unsigned||MUL|0||
|parent_id|int(10) unsigned|||0||
|change_date|datetime|YES||NULL||
|blocked|tinyint(3) unsigned|||0||
|priority|smallint(5) unsigned|||0||
|section|tinyint(3) unsigned|||0||
|url_alias|text|YES|MUL|NULL||
|path_string|varchar(255)|YES|MUL|NULL||
|depth|tinyint(3) unsigned|YES||NULL||
|main_node_id|int(10) unsigned||MUL|0||
|hidden|tinyint(4)|YES||0||

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

## sve_object_attributes - attributes for products and product groups

Corresponding to the class definition (see sve_class and sve_class_attributes), you can create multiple data fields for each entry in *sve_object_attributes*. Each of them can be set in its own language. The language codes are specified in ISO-639 (for example 'ger-DE' or 'eng-US'). 

Each entry consists of the following fields:

- attribute_id - ID of this attribute (see sve_class_attributes)
- node_id - internal node-ID
- data_int, data_float, data_text - value of the attribute depending on the data type (see sve_class_attributes)

### Table sve_object_attributes

|Field|Type|Null|Key|Default|Extra|
|--- |--- |--- |--- |--- |--- |
|node_id|int(10) unsigned||PRI|0||
|attribute_id|int(10) unsigned||PRI|0||
|data_float|float|YES||NULL||
|data_int|int(11)|YES||NULL||
|data_text|text|YES||NULL||
|language|varchar(6)||PRI|ger-DE||

### Table sve_class

This table describes all available classes. The class fields are defined in sve_class_attributes.

|Field|Type|Null|Key|Default|Extra|
|--- |--- |--- |--- |--- |--- |
|class_id|int(10) unsigned||PRI|0||
|class_name|varchar(255)|YES||NULL||
|name_identifier|int(10) unsigned|||0||

### Tabelle sve_class_attributes

The table describes the class fields.

|Field|Type|Null|Key|Default|Extra|
|--- |--- |--- |--- |--- |--- |
|attribute_id|int(10) unsigned||PRI|0||
|class_id|int(10) unsigned|||0||
|attribute_name|varchar(255)|YES||NULL||
|ezdatatype|varchar(255)|||||
|sort_field|varchar(255)|||data_text||

The following datatypes are supported:

#### ezstring

The data is stored in the field data_text as a flat string.

#### ezinteger

The data is stored in the field data_int.

#### ezprice

The price information is stored in the field data_float. The field data_text contains information about the VAT value in percent and a flag indicating if the price is incluing VAT or not.

Example:

``` code
+---------+--------------+------------+----------+-----------+----------+
| node_id | attribute_id | data_float | data_int | data_text | language |
+---------+--------------+------------+----------+-----------+----------+
|       5 |          210 |      0.952 |        0 | 19,1      | ger-DE   | 
+---------+--------------+------------+----------+-----------+----------+
```

- The price is 0.952 EUR (currency is defined per shop)
- The VAT is 19% and the price is including VAT).

#### ezmatrix (deprecated)

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

- columns: definition of the columns using a id and a Label
- rows: containing the data by rows and columns. if the table contains 2 rows a 2 columns 4 \<c\> Tags have to be generated!

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

### Table ses_externaldata

This table describes all external data from (SAP, PIM, TYP etc.). The content is encoded in json.  The class fields are defined in sve_class_attributes.

|Field|Type|Null|Key|Default|Extra|
|--- |--- |--- |--- |--- |--- |
|id|int(10) unsigned||PRI|||
|sku|varchar(40)|||||
|field_id|varchar(40)|||||
|language_code|varchar(8)|||||
|ses_field_type|varchar(20)|||||
|content|longtext||||json encoded|


Matching external data from "sve_class_attributes" of type "**ses_externaldata**" is done by data_text. It must match the "sku" (ex. 000000000000167738)
