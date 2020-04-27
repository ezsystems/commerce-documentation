# econtent - Configuration

### Important configuration for projects

The indexer for econtent needs to know which languages are used in your project and if you are using a fallback language.

Please adapt this setting according to your project needs:

``` 
# indexer configuration for econtent
siso_search.default.index_econtent_languages:
    ger-DE: eng-GB #language: ger-DE, fallback: eng-GB
    eng-GB: ger-DE #language: eng-GB, ger-DE
```

### Econtent Configuration

This is econtent.yml file explained 

```
#possible values: econtent or ez5
silver_eshop.default.catalog_data_provider: econtent
```

This will configure the shop to use econtent as data provider.

---

```
#definition for econtent languages
silver_econtent.default.languages: [ eng-GB, ger-DE]
```

This languages should are used for econtent. If there are other languages present in database they will be ignored.

---

```
#language for engl site access with no fallback.
silver_econtent.engl.languages: [ eng-GB ]
```

This is the language definition for the English site access.

---

```
#language for ger site access with english as fallback.
silver_econtent.ger.languages: [ ger-DE, eng-GB ]
```

This is the language definitions for the German site access. Please note that the second language specified here is the fallback language, this means that if a content is not found in the first language it will use the second language instead.

I.E.: There is a product which is defined in ger-DE, it has a name in ger-DE, but the description is only in eng-GB. With this configuration, the product will show and will be indexed as a ger-DE with eng-GB description.

---

```
silver_econtent.default.table_object: sve_object
silver_econtent.import.table_object: sve_object_tmp
silver_econtent.default.table_object_attributes: sve_object_attributes
silver_econtent.import.table_object_attributes: sve_object_attributes_tmp
silver_econtent.default.table_class: sve_class
silver_econtent.default.table_class_attributes: sve_class_attributes
silver_econtent.default.table_externaldata: ses_externaldata
```

These are the table names definition.
 
---
 
```
silver_econtent.default.section_filter: disabled
silver_econtent.default.table_catalog_filter: sve_object_catalog
silver_econtent.default.filter_SQL_where:
silver_econtent.default.catalog_filter_default_catalogcode:
```

Used for catalog segmentation: enabled or disabled

Defines the name of the table to be used
The where condition in order to  limit product catalog
A catalog segmentation code used for this siteaccess or by def

---

```
silver_econtent.default.root_node_id: 2
```

This is the root node id definition. Make sure your root node in save_object table has this id.

```
Example:
  
node_id class_id    parent_id   change_date blocked hidden  priority    section url_alias   path_string depth   main_node_id
2   1   0   NULL    0   0   1   1   Highlite    /2/ 1   2
```

---

```
#navigation configuration
silver_eshop.default.econtent_catalog_data_provider.filter:
 navigation:
 contentTypes: 1
        limit: 20
```

This will configure navigation.
contentTypes is the id specified in sve_class. In our examples 1 is for product groups and 2 is for products.
The limit is the amount to show in navigation service.

---

```
#configuration for the factory
silver_eshop.default.econtent_catalog_factory.product_group: createCatalogNode
silver_eshop.default.econtent_catalog_factory.product: createOrderableProductNode
```

These are the definition for the methods that actually create a catalog node.

---

```
silver_econtent.default.sku_id: 202
```

This is the definition for the field that will be used for SKU. This is required for building the catalog element. This is linked to `sve_class_attributes.attribute_id`

```
attribute_id    class_id    attribute_name  ezdatatype  sort_field
202 2   ses_sku ezstring    data_text
```

---

```
silver_econtent.default.class_id_catalog: 1
```

This will specify the id of product group elements found in se_class.class_id

```
class_id    class_name  name_identifier
1   product_group   101
```

---

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
 id: "ses_name"
 extract: "extractText"
 text:
 id: "text"
 extract: "extractText"
 image:
 id: "image"
 extract: "extractImage"
```

This defines how content is extracted from product groups.

---

```
silver_econtent.default.mapping.product:
    identifier:
        id: "node_id"
        extract: false
    parentElementIdentifier:
        id: "parent_id"
        extract: false
    url:
        id: "url_alias"
        extract: false
    sku:
        id: "ses_sku"
        extract: "extractText"
    manufacturerSku:
        id: "ses_manufacturer_sku"
        extract: "extractText"
    name:
        id: "ses_name"
        extract: "extractText"
    ean:
        id: "ses_ean"
        extract: "extractText"
    text:
        id: "ses_subtitle"
        extract: "extractText"
    subtitle:
        id: "ses_subtitle"
        extract: "extractText"
    image:
        id: "ses_image_main"
        extract: "extractImage"
    shortDescription:
        id: "ses_short_description"
        extract: "extractTextBlock"
    longDescription:
        id: "ses_long_description"
        extract: "extractTextBlock"
    price:
        id: "ses_unit_price"
        extract: "extractPrice"
    cacheIdentifier:
        id: "ses_sku"
        extract: "extractCacheIdentifier"
```

This defines how content is extracted from products.

extract specifies the method used to extract the value. If it is false the value will be extracted directly from db.

Image:

The image path to the product image should be relative to the path "web/var/assets/product_images" (the patch is stored in the config silver_eshop.default.catalog_factory.assets)

Specification data:

Specification data has to be stored in a json format (type ezstring) example (one group "technic" is used).

```
{
  "technic": [
    {
      "label": "Größe",
      "value": "1,69 cm (0.667 Zoll)"
    },
    {
      "label": "Kompatibilität",
      "value": "Epson Stylus Pro 9600, "
    }
  ]
}
```

Important: current the identifier in econtent has to be named "ses_specification"

The attributes of the specification data will be indexed in Solr as well.

---

```
# Definition for prefix ez fields stored in econtent
silver_econtent.default.ez_datatype_attribute_prefix: ezdata_
```

This definition is used by this plugin: SisoNavEcontentImporterPlugin

It defines the prefix of the field name that Ez Fields will have in econtent.

---

## Mapping of econtent fields

The factory maps content to attributes. It uses information from configuration file.

Configuration for mapping Econtent to fields was added to make Econtent Factory more flexibile. The mapping field (ex. Identifier) has to values inside:

1. id - which is the name of the attribute in Econtent (eg. node_id) or index in the externalData table (eg. typ.0.long_description)
2. extract - tells if data needs to be extracted. Possible values:  
      - `false - the data is not extracted (just set raw from database)`
      - `method name - data is extracted by the proper factory`
3. object - tell which object need to be created for return eg. TextBlockField

Ex. it maps "**node_id**" from content to "**identifier**" and does not extract it (just take raw from datamap)

```
silver_econtent.default.mapping.product:
    identifier:
        id: "node_id"
        extract: false
    parentElementIdentifier:
        id: "parent_id"
        extract: false
    url:
        id: "url_alias"
        extract: false
    sku:
        id: "ses_sku"
        extract: "extractText"
    manufacturerSku:
        id: "ses_manufacturer_sku"
        extract: "extractText"
    name:
        id: "ses_name"
        extract: "extractText"
    ean:
        id: "ses_ean"
        extract: "extractText"
    text:
        id: "ses_subtitle"
        extract: "extractText"
    subtitle:
        id: "ses_subtitle"
        extract: "extractText"
    image:
        id: "ses_image_main"
        extract: "extractImage"
    shortDescription:
        id: "ses_short_description"
        extract: "extractTextBlock"
    longDescription:
        id: "ses_long_description"
        extract: "extractTextBlock"
    price:
        id: "ses_unit_price"
        extract: "extractPrice"
    cacheIdentifier:
        id: "ses_sku"
        extract: "extractCacheIdentifier"
```

This defines how content is extracted from products.

extract specifies the method used to extract the value. If it is false the value will be extracted directly from db.

Image:

The image path to the product image should be relative to the path "web/var/assets/product_images" (the patch is stored in the config silver_eshop.default.catalog_factory.assets)

Specification data:

Specification data has to be stored in a json format (type ezstring) example (one group "technic" is used).

``` json
{
  "technic": [
    {
      "label": "Größe",
      "value": "1,69 cm (0.667 Zoll)"
    },
    {
      "label": "Kompatibilität",
      "value": "Epson Stylus Pro 9600, "
    }
  ]
}
```

Important: current the identifier in econtent has to be named "ses_specification"

The attributes of the specification data will be indexed in Solr as well.

## Data from an external data source (DB table **ses_externaldata)**

**EcontentCatalogFactory** for every CatalogElement collects all information from "**ses\_externaldata**" table for given sku. The sku is taken from "data\_text" column in "sve\_object\_attributes" table. It collects different data like pim, sap and typ codes. The it matches the external data with catalog element attributes and also puts all data in dataMap.

Configuration for **externalDataTypes**. It is used for searching different content in externalData table.

```
silver_econtent.default.externalDataType:
    pim:
        identifier: pim
    sap:
        identifier: sap
    typ:
        identifier: Typ1
        prefix: TY_
```
