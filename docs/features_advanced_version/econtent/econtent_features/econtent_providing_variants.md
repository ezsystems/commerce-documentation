# econtent - providing variants

The class attribute "ses_variants" can be used to store variant data for a product. 

The variants have to be stored in a JSON format. You will find more infos about variants here: [Product Variants](../../../developer_manual/catalog/catalog_feature_list/product_variants/product_variants.md)

!!! note

    Please note that the format of the JSON will be changed in the near future (2019) in order to be more generic.

``` json
[
  {
    "sku": {
      "label": "Sku",
      "value": "0002"
    },
    "variantCode": {
      "label": "Variant Code",
      "value": "D4142"
    },
    "description": {
      "label": "Description",
      "value": "Bubble light bulb small"
    },
    "characteristicCode1": {
      "label": "small",
      "value": "small"
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
      "value": "0002"
    },
    "variantCode": {
      "label": "Variant Code",
      "value": "D4143"
    },
    "description": {
      "label": "Description",
      "value": "Bubble light bulb large"
    },
    "characteristicCode1": {
      "label": "large",
      "value": "large"
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
      "value": "0002"
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
      "label": 30,
      "value": 30
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
