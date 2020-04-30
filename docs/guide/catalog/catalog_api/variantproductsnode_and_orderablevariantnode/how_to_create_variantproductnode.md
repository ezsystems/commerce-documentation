# How to create VariantProductNode

Usually you need to create a VariantProductNode in your factory. The most easy way to explain how to do it, is a simple code example:

``` php
public function createVariantProductNode($rawData = null)
{      

    if ($rawData == null || count($rawData) === 0) {
        throw new CatalogFactoryRawDataIsMalformedException('Argument $rawData is empty.');
    }

    /** @var $contentObject ContentInfo */
    $contentObject = $rawData['contentInfo'];

    /*$languageCode = is_array($rawData['languages']) && isset($rawData['languages'][0])
        ? $rawData['languages'][0] : null;*/

    /** @var $dataMap array */
    $dataMap = $contentObject['data_map'];

    $attributes = $this->mapAttributesFromContent($contentObject, 'product');
    if(!isset($attributes['vatCode']) || empty($attributes['vatCode'])) {
        $vatCode = $this->configResolver->getParameter('standard_price_factory.fallback_vat_code', 'siso_core');
        $attributes['vatCode'] = $vatCode;
    }
    $attributes['url'] = '/' . $attributes['url'];

    $variantCodes = array(
        '4:3:black' => array(
            '1' => 'black',
            '2' => '4:3'
        ),
        '4:3:white' => array(
            '1' => 'white',
            '2' => '4:3'
        ),
        '16:9:black' => array(
            '1' => 'black',
            '2' => '16:9'
        ),
        '16:10:white' => array(
            '1' => 'white',
            '2' => '16:10'
        )
    );

    $characteristics = array(
        1 => array(
            'label' => 'Color',
            'codes'  => array(
                'black' => 'Black',
                'white' => 'White',
            ),
        ),
        2 => array(
            'label' => 'Size',
            'codes'  => array(
                '4:3' => '4:3',
                '16:9' => '16:9',
                '16:10' => '16:10'
            ),
        )
    );

    $variantAttributes = array(
        '4:3:black' => array(
            'characteristicCode1' => new TextLineField(array('text' => 'Black')),
            'characteristicLabel1' => new TextLineField(array('text' => 'black')),
            'price' => new PriceField(array(
                'price' => $this->priceFactoryService->createPrice(array(
                    'price' => (float)207.26,
                    'isVatPrice' => true,
                    'vatPercent' => 19.0,
                    'currency' => 'EUR',
                    'source' => 'econtent'
                ))
            )),
            'stock' => new StockField(array(
                'stockNumeric' => 1
            ))
        ),
        '4:3:white' => array(
            'characteristicCode1' => new TextLineField(array('text' => 'White')),
            'characteristicLabel1' => new TextLineField(array('text' => 'white')),
            'price' => new PriceField(array(
                'price' => $this->priceFactoryService->createPrice(array(
                    'price' => (float)207.06,
                    'isVatPrice' => true,
                    'vatPercent' => 19.0,
                    'currency' => 'EUR',
                    'source' => 'econtent'
                ))
            )),
            'stock' => new StockField(array(
                'stockNumeric' => 2
            ))
        ),
        '16:9:black' => array(
            'characteristicCode1' => new TextLineField(array('text' => 'Black')),
            'characteristicLabel1' => new TextLineField(array('text' => 'black')),
            'price' => new PriceField(array(
                'price' => $this->priceFactoryService->createPrice(array(
                    'price' => (float)227.06,
                    'isVatPrice' => true,
                    'vatPercent' => 19.0,
                    'currency' => 'EUR',
                    'source' => 'econtent'
                ))
            )),
            'stock' => new StockField(array(
                'stockNumeric' => 30
            ))
        ),
        '16:10:white' => array(
            'characteristicCode1' => new TextLineField(array('text' => 'White')),
            'characteristicLabel1' => new TextLineField(array('text' => 'white')),
            'price' => new PriceField(array(
                'price' => $this->priceFactoryService->createPrice(array(
                    'price' => (float)228.06,
                    'isVatPrice' => true,
                    'vatPercent' => 19.0,
                    'currency' => 'EUR',
                    'source' => 'econtent'
                ))
            )),
            'stock' => new StockField(array(
                'stockNumeric' => null
            ))
        )
    );

    $variantCharacteristics = new SimpleVariantCharacteristics(
        array(
            'characteristics' => $characteristics,
            'variantCodes' => $variantCodes,
            'variantAttributes' => $variantAttributes
        )
    );

    $variantAttributes = array(
        'variantCharacteristics' => $variantCharacteristics,
    );

    foreach($variantAttributes as $index => $variantAttribute){
        $attributes[$index] = $variantAttribute;
    }

    $productNode = new VariantProductNode($attributes, $this->urlService);

    return $this->fillCatalogElementDataMap($productNode, $dataMap);
}
```
