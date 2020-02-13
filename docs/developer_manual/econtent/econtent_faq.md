#  econtent - FAQ 

After using econtent the top navigation is empty

1\) Please check your data inside econtent:

  - Make sure that for all objects the attribute which is defined as name\_identifier in sve\_class is stored in sve\_object\_attributes\!

    ``` 
    select * from sve_class;
    +----------+---------------+-----------------+
    | class_id | class_name    | name_identifier |
    +----------+---------------+-----------------+
    |        1 | product_group |             101 |
    |        2 | product       |             205 |
    +----------+---------------+-----------------+
     
     select * from sve_object_attributes where node_id=1003;
    +---------+--------------+----------+------------+----------+--------------------------------------------+
    | node_id | attribute_id | language | data_float | data_int | data_text                                  |
    +---------+--------------+----------+------------+----------+--------------------------------------------+
    |    1003 |          202 | ger-DE   |       NULL |     NULL | 80810                                      |
    |    1003 |          205 | ger-DE   |       NULL |     NULL | Projection Bulb EFP GZ6.35 Philips12V 100W |
    |    1003 |          209 | ger-DE   |       13.5 |     NULL | NULL                                       |
    +---------+--------------+----------+------------+----------+--------------------------------------------+
    ```

2\) Check if the root element also has at least this data inside the sve\_object\_attributes table

``` 
select * from sve_object_attributes where node_id=2;
+---------+--------------+----------+------------+----------+-----------+
| node_id | attribute_id | language | data_float | data_int | data_text |
+---------+--------------+----------+------------+----------+-----------+
|       2 |          101 | ger-DE   |       NULL |     NULL | Produkte  |
+---------+--------------+----------+------------+----------+-----------+
```

3\) check the settings

Check if the correct class id is set in the configuration:

  - check if types contains the correct class\_id used in econtent for product\_groups. Ir is usually 1

  - check if the fields used for the labels are set:
    
        label_fields

``` 
siso_core.default.navigation.catalog:
        #the class id has to be specified here
        types: ['1']
        sections: [1, 2]
        enable_priority_zero: true
        label_fields: ['ses_category_ses_name_value_s','name_s']
        additional_fields: ['ses_category_ses_code_value_s', 'ses_category_ses_name_value_s' ]
```

Check if the field mapping is correct (the same has to be done for products: silver\_econtent.default.mapping.product):

``` 
 silver_econtent.default.mapping.product_group:
        identifier:
            id: "node_id"
            extract: false
        parentElementIdentifier:
            id: "parent_id"
            extract: false
        url:
            id: "url_alias"
            extract: false
        name:
            id: "name"
            extract: "extractText"
```

My product are not displayed in the detail page

Please make sure that the main attributes are defined at least such as:

  - name\_identifier (see above)
  - ses\_sku
  - ses\_price 

The search say "XX" product found but none of them are displayed

Please check if you have issues while building the catalogElements.

In dev mode exceptions are logged in silver.eshop.log

Getting a product by sku seems to be slow (e.g. in product lists). How can this be improved?

Doctrine does not allow to create a index using a part of a string. This is why it might be helpful to setup a index manually:

``` 
create index ind_datatext  on sve_object_attributes (data_text(20));
create index ind_datatext  on sve_object_attributes_tmp (data_text(20));
```

In this case we assume that a sku has a max lenght of 20 which should be fine for almost all projects.

Facets are lowercased

The configuration index_facet_fields has to be defined for the specific fields which should not be lowercased. See Indexing econtent data
