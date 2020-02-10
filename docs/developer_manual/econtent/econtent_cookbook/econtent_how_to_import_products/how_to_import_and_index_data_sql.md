#  How to import and index data (SQL) 

### See also [dedicated solr cores](Dedicated-solr-cores-for-econtent_23560727.html)

### Econtent Database Tables

There are 5 database tables:

<table>
<thead>
<tr class="header">
<th><pre><code>table</code></pre></th>
<th><pre><code>purpose</code></pre></th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>sve_class</code></pre></td>
<td><pre><code>Stores types of catalog elements (ex. product, product_group). Every type has an identifier, which has a relation with sve_class_attributes</code></pre>
<p>Example:</p>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>class_id (pk)</th>
<th>class_name</th>
<th>name_identifier</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td><pre><code>product_group</code></pre></td>
<td><pre><code>101</code></pre></td>
</tr>
<tr>
<td>2</td>
<td><pre><code>product</code></pre></td>
<td>201</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td><pre><code>sve_class_attributes</code></pre></td>
<td><pre><code>Stores all possible catalog element attributes (name, type) for different catalog elements (class_id)</code></pre>
<p>Example:</p>

<table>
<thead>
<tr class="header">
<th>attribute_id (pk)</th>
<th>class_id (fk)</th>
<th>attribute_name</th>
<th>ezdatatype</th>
<th>sort_field</th>
</tr>
</thead>
<tbody>
<tr>
<td>101</td>
<td>1</td>
<td>ses_name</td>
<td>ezstring</td>
<td>data_text</td>
</tr>
<tr>
<td>201</td>
<td>2</td>
<td>ses_name</td>
<td>ezstring</td>
<td>data_text</td>
</tr>
<tr>
<td>202</td>
<td>2</td>
<td>ses_sku</td>
<td>ezstring</td>
<td>data_text</td>
</tr>
</tbody>
</table>

<p>As a convention, attributes that belong to class_id start with class_id first digit. Like 1 =&gt; 101 or 2 +. 202</p>
<p><br />
</p></td>
</tr>
<tr>
<td><pre><code>sve_object</code></pre>
<p><br />
</p></td>
<td><pre><code>Stores all catalog elements and general information about them (url, parent, depth etc)</code></pre>
<p>Example:</p>

<table style="width:100%;">
<colgroup>
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
<col style="width: 7%" />
</colgroup>
<thead>
<tr class="header">
<th><br />
</th>
<th>node_id (pk)</th>
<th>class_id (fk)</th>
<th>parent_id</th>
<th>change_date</th>
<th>blocked</th>
<th>hidden</th>
<th>priority</th>
<th>section</th>
<th>url_alias</th>
<th>path_string</th>
<th>depth</th>
<th>main_node_id</th>
</tr>
</thead>
<tbody>
<tr>
<td>This is the root element</td>
<td>2</td>
<td>1</td>
<td>0</td>
<td>2013-09-15 12:55:29</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>Highlite</td>
<td>/2/</td>
<td>1</td>
<td>2</td>
</tr>
<tr>
<td>This is some product</td>
<td>454</td>
<td>2</td>
<td>2</td>
<td>2013-09-15 12:55:29</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>Highlite/Proscreen-electric_7</td>
<td>/2/454/</td>
<td>2</td>
<td>454</td>
</tr>
</tbody>
</table>

<p><br />
</p>
<p><br />
</p></td>
</tr>
<tr>
<td><pre><code>sve_object_attributes</code></pre></td>
<td><pre><code>Stores all attributes for the given catalog element depending on language.</code></pre>
<p>Example:</p>

<table>
<thead>
<tr class="header">
<th>node_id (fk)</th>
<th>attribute_id (fk)</th>
<th>data_float</th>
<th>data_int</th>
<th>data_text</th>
<th>language</th>
</tr>
</thead>
<tbody>
<tr>
<td>2</td>
<td>101</td>
<td>NULL</td>
<td>NULL</td>
<td>Highlight</td>
<td>eng-GB</td>
</tr>
<tr>
<td>454</td>
<td>201</td>
<td>NULL</td>
<td>NULL</td>
<td>Proscreen Electrit 7</td>
<td>eng-GB</td>
</tr>
</tbody>
</table>

<p><br />
</p>
<p><br />
</p></td>
</tr>
<tr>
<td><pre><code>ses_externaldata</code></pre></td>
<td><pre><code>Stores additional information for sve_object_attributes of &quot;ses_externaldata&quot;. It collect more information for that kind of catalog element.</code></pre></td>
</tr>
</tbody>
</table>

**Please note that although tables have relationships, there are no constraints defined in database definition.**

### Examples queries

This queries are could be useful for testing imported data or debuging.

``` 
/* Get all elements with all tables joined: */
SELECT
    * 
FROM
    sve_object 
JOIN
    sve_object_attributes ON ( sve_object.node_id = sve_object_attributes.node_id ) 
JOIN
    sve_class_attributes ON ( sve_object_attributes.attribute_id = sve_class_attributes.attribute_id );
 
/* Get all products */
SELECT
    * 
FROM
    sve_object 
JOIN
    sve_object_attributes ON ( sve_object.node_id = sve_object_attributes.node_id ) 
JOIN
    sve_class_attributes ON ( sve_object_attributes.attribute_id = sve_class_attributes.attribute_id )
WHERE
    sve_object.class_id = 2 /* Class id 2 is for products */;
 
/* Get all product groups */
SELECT
    * 
FROM
    sve_object 
JOIN
    sve_object_attributes ON ( sve_object.node_id = sve_object_attributes.node_id ) 
JOIN
    sve_class_attributes ON ( sve_object_attributes.attribute_id = sve_class_attributes.attribute_id )
WHERE
    sve_object.class_id = 1 /* Class id 1 is for product groups*/ ;

/* Get product node ids and names*/
SELECT
    sve_object.node_id,
    sve_object_attributes.data_text
FROM
    sve_object 
JOIN
    sve_object_attributes ON ( sve_object.node_id = sve_object_attributes.node_id ) 
JOIN
    sve_class_attributes ON ( sve_object_attributes.attribute_id = sve_class_attributes.attribute_id )
WHERE
    sve_object.class_id = 2 
AND
    sve_class_attributes.attribute_id = 201
ORDER BY
    sve_object_attributes.data_text;
 
```

### Import process for econtent

As you may noticed, tables sve\_class and sve\_class\_attributes have data definitions and sve\_object and sve\_object\_attributes have the data itself.

sve\_class and sve\_class\_attributes import example:

    This tables should be modified only when data definition is changed.

``` 
/* Insert data in sve_class table */
 
INSERT INTO `sve_class` (`class_id`, `class_name`, `name_identifier`)
VALUES
    (1,'product_group',101),
    (2,'product',201);
 
/* Insert data in sve_class_attributes table */
INSERT INTO `sve_class_attributes` (`attribute_id`, `class_id`, `attribute_name`, `ezdatatype`, `sort_field`)
VALUES
    (101,1,'ses_name','ezstring','data_text'),
    (102,1,'number','ezstring','data_text'),
    (103,1,'code','ezstring','data_text'),
    (201,2,'ses_name','ezstring','data_text'),
    (202,2,'ses_sku','ezstring','data_text'),
    (203,2,'product_sort_field','ezstring','data_text'),
    (204,2,'sub_headline','ezstring','data_text'),
    (205,2,'marker','ezstring','data_text'),
    (206,2,'length','ezfloat','data_float');
 
/* This tables should be modified only when data definition is changed. */
```

sve\_object and sve\_object\_attributes import examples:

``` 
/* Insert data in sve_object table */
INSERT INTO `sve_object` (`node_id`, `class_id`, `parent_id`, `change_date`, `blocked`, `hidden`, `priority`, `section`, `url_alias`, `path_string`, `depth`, `main_node_id`)
VALUES
    (2,1,0,NULL,0,0,1,1,'Highlite','/2/',1,2),
    (11,1,2,NULL,0,0,1,1,'Highlite/DMT','/2/11/',2,11),
    (1000001,1,11,NULL,0,0,1,1,'Highlite/DMT/New-Products','/2/11/1000001/',3,1000001),
    (1000101,1,11,NULL,0,0,1,1,'Highlite/DMT/Outlet-Sales','/2/11/1000101/',3,1000101),
    (25290,1,11,NULL,0,0,1,1,'Highlite/DMT/Media-Gear','/2/11/25290/',3,25290),
    (125290,1,25290,NULL,0,0,1,1,'Highlite/DMT/Media-Gear/Screens','/2/11/25290/125290/',4,125290),
    (225290,2,125290,NULL,0,0,1,1,'Highlite/DMT/Media-Gear/Screens/Proscreen-manual','/2/11/25290/125290/225290/',5,225290),
    (225291,2,125290,NULL,0,0,2,1,'Highlite/DMT/Media-Gear/Screens/Proscreen-manual_1','/2/11/25290/125290/225291/',5,225291),
    (220841,2,125290,NULL,0,0,3,1,'Highlite/DMT/Media-Gear/Screens/Projection-Screen-4-3','/2/11/25290/125290/220841/',5,220841),
    (225292,2,125290,NULL,0,0,4,1,'Highlite/DMT/Media-Gear/Screens/Proscreen-manual_2','/2/11/25290/125290/225292/',5,225292),
    (220842,2,125290,NULL,0,0,5,1,'Highlite/DMT/Media-Gear/Screens/Projection-Screen-4-3-Manual','/2/11/25290/125290/220842/',5,220842);
 
/* Insert data in sve_obect_attributes table */
 
INSERT INTO `sve_object_attributes` (`node_id`, `attribute_id`, `data_float`, `data_int`, `data_text`, `language`)
VALUES
    (2,101,NULL,NULL,'Highlite','eng-GB'),
    (11,101,NULL,NULL,'DMT','eng-GB'),
    (11,102,NULL,NULL,'00.10','eng-GB'),
    (11,102,NULL,NULL,'00.10','ger-DE'),
    (1000001,101,NULL,NULL,'Neuheiten','ger-DE'),
    (1000001,101,NULL,NULL,'New Products','eng-GB'),
    (1000001,102,NULL,NULL,'00.00','eng-GB'),
    (1000001,103,NULL,NULL,'NEW','eng-GB'),
    (1000101,101,NULL,NULL,'Auslaufartikel','ger-DE'),
    (1000101,101,NULL,NULL,'Outlet Sales','eng-GB'),
    (1000101,102,NULL,NULL,'ZZZ','eng-GB'),
    (1000101,103,NULL,NULL,'DECLINE','eng-GB'),
    (25290,101,NULL,NULL,'Media Gear','eng-GB'),
    (125290,101,NULL,NULL,'Screens','eng-GB'),
    (125290,102,NULL,NULL,'00.10.020','eng-GB'),
    (125290,102,NULL,NULL,'00.10.020','ger-DE'),
    (225290,201,NULL,NULL,'Proscreen manual','eng-GB'),
    (225290,202,NULL,NULL,'100350','eng-GB'),
    (225290,203,NULL,NULL,'00.10.020/005','eng-GB'),
    (225290,203,NULL,NULL,'00.10.020/005','ger-DE'),
    (225290,204,NULL,NULL,'72\" 4:3','eng-GB'),
    (225290,204,NULL,NULL,'72\" 4:3','ger-DE');
 
 
```

Please do not forget to index the products in solr. Details see: [econtent - Staging system](econtent---Staging-system_23561079.html) and [Indexing econtent data](Indexing-econtent-data_23560588.html)

### Import procedure using temp tables

Tables sve\_object and sve\_object\_attributes have 2 clone temp tables called sve\_object\_tmp and sve\_object\_attributes\_tmp. These tmp tables are used to import new data and keep data available while importing using the normal tables. After import is finished you can execute a command to rename tmp tables to normal tables:

    php bin/console silversolutions:econtent-tables-swap

### Languages

The language definitions of econtent elements are defined in se\_object\_attributes. Every attribute row have a language definition.

Example:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><div class="container" title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<code class="sourceCode php">node_id attribute_id    data_float  data_int    data_text     language</code>

<div class="line number2 index1 alt1">
<code class="sourceCode php"><span class="dv">1000001 <span class="dv">101             <span class="kw">NULL        <span class="kw">NULL        Neuheiten     ger-<span class="kw">DE</code>

<div class="line number3 index2 alt2">
<code class="sourceCode php"><span class="dv">1000001 <span class="dv">101             <span class="kw">NULL        <span class="kw">NULL        <span class="kw">New Products  eng-<span class="kw">GB</code>

</td>
</tr>
</tbody>
</table>

There is no language hierarchy. But please keep in mind that only languages specified in configuration will be indexed:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><pre><code>siso_search.default.index_econtent_languages: [ ger-DE, eng-GB ]</code></pre></td>
<td>This will specify the languages to index.</td>
</tr>
</tbody>
</table>

### Econtent Indexer

For instructions about how to index econtent you can use this link: [Indexing econtent data](http://confluence.extranet.silversolutions.de:8090/display/EX/Indexing+econtent+data)

For instructions for how to create custom plugins for indexing you can view this cookbook: [Cookbook - How to implement a custom indexer plugin for econtent](http://confluence.extranet.silversolutions.de:8090/display/EX/Cookbook+-+How+to+implement+a+custom+indexer+plugin+for+econtent)

### Ez Attributes in econtent

If there is the need to enrich an econtent site with ez data there is a command from the following plugin that can be used for that: [SisoNavEcontentImporterPlugin](#)
