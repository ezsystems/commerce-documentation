#  How to export products 

Get the products from the catalog provider (maximum 100 products and a depth of 10):

``` 
        $catalogService = $this->getContainer()->get('silver_catalog.data_provider_service');
        $repository = $this->getContainer()->get('ezpublish.api.repository');
 
        // use sudo to get all product in depended from roles and rights
        $productList = $repository->sudo(
            function () use ($catalogService) {
                return $catalogService->getDataProvider()
                    ->fetchChildrenList(null, 10, array('filterType' => 'productList'), null, 0, 100);
            }
        );
 
        
```

Export the products:

``` 
        
 
        $html5converter = $this->getContainer()->get('ezpublish.fieldtype.ezxmltext.converter.html5');
        
        foreach ($productList as $product) {
            $line = array();

            /** @var $product \Silversolutions\Bundle\EshopBundle\Product\ProductNode */

            $line[] = $product->sku;
            $line[] = $product->type;
            $line[] = $product->ean;
            $line[] = $product->name;
            $line[] = strip_tags($html5converter->convert($product->shortDescription->text));
            $line[] = strip_tags($html5converter->convert($product->longDescription->text));
            $line[] = (isset($product->imageList[0]))?$product->imageList[0]->fileName:'';
            $line[] = $product->minOrderQuantity;
            $line[] = $product->maxOrderQuantity;
            $line[] = $product->packagingUnit;
            $line[] = $product->unit;
            $line[] = $product->vatCode;
            $line[] = $product->price->price->priceInclVat;
            $line[] = $product->stock->stockNumeric;
            $text .= implode(';', $line) .PHP_EOL;
        }
        
```
