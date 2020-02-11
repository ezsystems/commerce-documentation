#  VariantType 

The VariantType Fieldtype offers a user interface for editing variants. It stores the information about the variants in a json format.

The FieldType offers a selection of preconfigured varianttypes.

![](attachments/23560699/23571060.png)

A variant type can be a 1level or 2level variant. The variant types offered can be setup in a yml file: 

``` 
siso_core.default.variant_types:
    -
        id: size_color
        type_label: "Size and Color"
        variant_levels: 2
        variant_level_label_1: "Size"
        variant_level_code_1: "size"
        variant_level_code_2: "color"
        variant_level_label_2: "Color"
    -
        id: size
        type_label: "Size"
        variant_levels: 1
        variant_level_label_1: "Size"
        variant_level_code_1: "size"
    -
        id: color
        type_label: "color"
        variant_levels: 1
        variant_level_label_1: "Color"
        variant_level_code_1: "color"
    -
        id: package
        type_label: "Package"
        variant_levels: 1
        variant_level_label_1: "Package size"
        variant_level_code_1: "packagesize"

```

Depending on the variant type a edit form is generated:

![](attachments/23560699/23562958.png)

The data is stored in a JSON format:

``` 
[
  {
    "sku": {
      "label": "Sku",
      "value": "5515"
    },
    "variantCode": {
      "label": "Variant Code",
      "value": "5515"
    },
    "description": {
      "label": "Description",
      "value": "Alibaba Single Door Silver Refrigerator"
    },
    "characteristicCode1": {
      "label": "22\"",
      "value": "22\""
    },
    "characteristicLabel1": {
      "label": "Color",
      "value": "Color"
    },
    "characteristicCode2": {
      "label": "Silver",
      "value": "Silver"
    },
    "characteristicLabel2": {
      "label": "",
      "value": ""
    },
    "priceNet": {
      "label": "Listprice net",
      "value": "220"
    },
    "vatPercent": {
      "label": "VAT percent",
      "value": ""
    },
    "dataMap_countryOfOrigin": {
      "label": "dataMap Country of Origin",
      "value": ""
    }
  },
  {
    "sku": {
      "label": "Sku",
      "value": "5515"
    },
    "variantCode": {
      "label": "Variant Code",
      "value": ""
    },
    "description": {
      "label": "Description",
      "value": "Alibaba Single Door Silver Refrigerator"
    },
    "characteristicCode1": {
      "label": "22\"",
      "value": "22\""
    },
    "characteristicLabel1": {
      "label": "Size",
      "value": "Size"
    },
    "characteristicCode2": {
      "label": "Silver",
      "value": "Silver"
    },
    "characteristicLabel2": {
      "label": "Color",
      "value": "Color"
    },
    "priceNet": {
      "label": "Listprice net",
      "value": "220"
    },
    "vatPercent": {
      "label": "VAT percent",
      "value": ""
    },
    "dataMap_countryOfOrigin": {
      "label": "dataMap Country of Origin",
      "value": ""
    }
  },
  {
    "sku": {
      "label": "Sku",
      "value": "5515"
    },
    "variantCode": {
      "label": "Variant Code",
      "value": "5532"
    },
    "description": {
      "label": "Description",
      "value": "Alibaba Single Door Silver Refrigerator"
    },
    "characteristicCode1": {
      "label": "30\"",
      "value": "30\""
    },
    "characteristicLabel1": {
      "label": "Size",
      "value": "Size"
    },
    "characteristicCode2": {
      "label": "White",
      "value": "White"
    },
    "characteristicLabel2": {
      "label": "Color",
      "value": "Color"
    },
    "priceNet": {
      "label": "Listprice net",
      "value": "330"
    },
    "vatPercent": {
      "label": "VAT percent",
      "value": ""
    },
    "dataMap_countryOfOrigin": {
      "label": "dataMap Country of Origin",
      "value": ""
    }
  }
]
```

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-7-17\_19-2-42.png](attachments/23560699/23571060.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-17\_19-5-9.png](attachments/23560699/23571061.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-11-6\_9-4-11.png](attachments/23560699/23562958.png) (image/png)  
