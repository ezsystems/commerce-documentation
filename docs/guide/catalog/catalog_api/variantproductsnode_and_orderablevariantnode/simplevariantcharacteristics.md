# SimpleVariantCharacteristics

`SimpleVariantCharacteristics` implements `VariantCharacteristicsInterface` and contains all necessary data using three associated arrays:

``` php
private $characteristics;

//structure looks like
$characteristics = array(
    'Color' => array(
        'label' => 'Color',
        'type' => 'dropdown',
        'images' => array(
              'grn' => new ImageField(),
              'blue' => new ImageField(),
          ),
        'codes' => array(
            'grn' => 'Green',
            'blue' => 'Blue'
        )
    ),
    'Unit' => array(
        'label' => 'Unit',
        'type' => 'dropdown',
        'codes' => array(
            'box' => 'Box',
            'pcs' => 'Piece'
        )
    )
);

private $variantCodes;

//structure looks like
$variantCodes = array(
    'grn-box' => array(
        'Color' => 'grn',
        'Unit' => 'box'
    ),
    'blue-pcs' => array(
        'Color' => 'blue',
        'Unit' => 'pcs'
    )
);

private $variantAttributes;

/*
* This characteristic needs to know which of the variant attributes are characteristic Fields. 
* That is why attributes are displayed in b2b need to follow special rules:
*     - characteristicCodeXXX
*     - characteristicLabelXXX
*
* Everything else is additional data like dataMap, price, stock.
*/
$variantAttributes = array(
    'grn-box' => array(
        'characteristicCodeColor'    => FieldInterface,
        'characteristicLabelColor'   => FieldInterface,
        'characteristicCodeUnit'     => FieldInterface,
        'characteristicLabelUnit'    => FieldInterface,
        'description' => TextLineField,
        'price' => PriceField,
        'dataMap' => array(
            'countryOfOrigin' => Spain
        )
     ),
    'blue-pcs' => array(
        'characteristicCodeColor'    => FieldInterface,
        'characteristicLabelColor'   => FieldInterface,
        'characteristicCodeUnit'     => FieldInterface,
        'characteristicLabelUnit'    => FieldInterface,
        'description' => TextLineField,
        'price' => PriceField,
        'dataMap' => array(
            'countryOfOrigin' => Italy
        )
    )
);
```

To access the all variant codes from the template, use [Variant Services](variant_services.md).
