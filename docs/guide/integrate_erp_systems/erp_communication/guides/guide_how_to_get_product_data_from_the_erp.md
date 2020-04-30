# Guide - how to get product data from the ERP

## How to retrieve products from ERP in a Symfony controller

``` php
/** @var WebConnectorErpService $importer */
$importer = $this->get('silver_erp.facade');
//Starting SKU
$startSku = '1..';
//Number of items to be retrieved (optional, default is 10)
$itemMaxCount = 30;
$items = $importer->selectItem($startSku, $itemMaxCount);
foreach($items as $products)
{
    foreach($products as $product)
    {
        //Prints the product's name
        echo (string)$product->ProductMeta->name->TextField->text->translations->translation[0]->value;
        //Prints the product's price
        echo (string)$product->ProductMeta->price->PriceField->price->price->value;
    }
}
```

??? note "Example output of selectItem method"

    ``` 
    Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponse Object
    (
        [Item] => Array
            (
                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItem Object
                    (
                        [ProductMeta] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMeta Object
                            (
                                [priority] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [visibility] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [sku] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 3123
                                    )
                                [manufacturerSku] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [ean] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [name] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetaname Object
                                    (
                                        [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                            (
                                                [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                    (
                                                        [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                            (
                                                                [translation] => Array
                                                                    (
                                                                        [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                            (
                                                                                [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                    (
                                                                                        [value] => ger-DE
                                                                                    )
                                                                                [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                    (
                                                                                        [value] => Gymnastikmatte RFM aufrollbar, blau 180x80x1,5cm
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                    )
                                                [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                    (
                                                        [value] => 
                                                    )
                                            )
                                    )
                                [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetatext Object
                                    (
                                        [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                            (
                                                [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                    (
                                                        [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                            (
                                                                [translation] => Array
                                                                    (
                                                                    )
                                                            )
                                                    )
                                                [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                    (
                                                        [value] => 
                                                    )
                                            )
                                    )
                                [type] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [isOrderable] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [price] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetaprice Object
                                    (
                                        [PriceField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PriceField Object
                                            (
                                                [price] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PriceFieldprice Object
                                                    (
                                                        [price] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [priceInclVat] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [priceExclVat] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [isVatPrice] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [vatPercent] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [currency] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [discount] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [campaign] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [source] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [quantityDiscount] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                                [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                    (
                                                        [value] => 
                                                    )
                                            )
                                    )
                                [stock] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetastock Object
                                    (
                                        [StockField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StockField Object
                                            (
                                                [stockNumeric] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => 
                                                    )
                                                [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                    (
                                                        [value] => 
                                                    )
                                            )
                                    )
                                [shortDescription] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetashortDescription Object
                                    (
                                        [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                            (
                                                [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                    (
                                                        [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                            (
                                                                [translation] => Array
                                                                    (
                                                                    )
                                                            )
                                                    )
                                                [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                    (
                                                        [value] => 
                                                    )
                                            )
                                    )
                                [sesLongDescription] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetasesLongDescription Object
                                    (
                                        [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                            (
                                                [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                    (
                                                        [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                            (
                                                                [translation] => Array
                                                                    (
                                                                    )
                                                            )
                                                    )
                                                [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                    (
                                                        [value] => 
                                                    )
                                            )
                                    )
                                [unit] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => STK
                                    )
                                [packaging_unit] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [minOrderQuantity] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [maxOrderQuantity] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [allowedQuantity] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [vatCode] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                    (
                                        [value] => 
                                    )
                                [sesImage] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetasesImage Object
                                    (
                                        [ImageField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageField Object
                                            (
                                                [alternativeText] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageFieldalternativeText Object
                                                    (
                                                        [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageFieldalternativeTexttranslations Object
                                                            (
                                                                [translation] => Array
                                                                    (
                                                                    )
                                                            )
                                                    )
                                                [fileName] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageFieldfileName Object
                                                    (
                                                        [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                        [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageFieldfileNametranslations Object
                                                            (
                                                                [translation] => Array
                                                                    (
                                                                    )
                                                            )
                                                    )
                                                [fileSize] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => 
                                                    )
                                                [path] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => 
                                                    )
                                                [id] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => 
                                                    )
                                                [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                    (
                                                        [value] => 
                                                    )
                                            )
                                    )
                            )
                        [GroupMeta] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMeta Object
                            (
                                [GroupLocations] => Array
                                    (
                                        [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocations Object
                                            (
                                                [Location] => Array
                                                    (
                                                        [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocation Object
                                                            (
                                                                [code] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [priority] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 1
                                                                    )
                                                                [visibility] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 1
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocationtranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocationtranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => Gruppe ..
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [1] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocation Object
                                                            (
                                                                [code] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [priority] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 1
                                                                    )
                                                                [visibility] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 1
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocationtranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocationtranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => Kategorie_..
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                    )
                                            )
                                    )
                            )
                        [Datamap] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamap Object
                            (
                                [Image] => Array
                                    (
                                    )
                                [Array] => Array
                                    (
                                        [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_long_description
                                                    )
                                                [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                    (
                                                        [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [1] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_attributes
                                                    )
                                                [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                    (
                                                        [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                            (
                                                                                                [value] => Array
                                                                                                    (
                                                                                                        [Attributes] => Array
                                                                                                            (
                                                                                                                [0] => Array
                                                                                                                    (
                                                                                                                        [Code] => ARTIKELART
                                                                                                                        [Label] => Artikelart
                                                                                                                        [Value] => Lagerartikel
                                                                                                                    )
                                                                                                                [1] => Array
                                                                                                                    (
                                                                                                                        [Code] => EINHEIT
                                                                                                                        [Label] => Einheit
                                                                                                                        [Value] => STK
                                                                                                                    )
                                                                                                                [2] => Array
                                                                                                                    (
                                                                                                                        [Code] => FARBE
                                                                                                                        [Label] => Farben
                                                                                                                        [Value] => BLAU
                                                                                                                    )
                                                                                                                [3] => Array
                                                                                                                    (
                                                                                                                        [Code] => KRED.-ART-NR.
                                                                                                                        [Label] => Lieferanten Artikelnr.
                                                                                                                        [Value] => 03625232
                                                                                                                    )
                                                                                                                [4] => Array
                                                                                                                    (
                                                                                                                        [Code] => LIEFERANT
                                                                                                                        [Label] => Lieferant
                                                                                                                        [Value] => Rehaforum Medical GmbH
                                                                                                                    )
                                                                                                                [5] => Array
                                                                                                                    (
                                                                                                                        [Code] => MWST
                                                                                                                        [Label] => Mehrwertsteuer
                                                                                                                        [Value] => 19 %
                                                                                                                    )
                                                                                                            )
                                                                                                    )
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [2] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_scaled_prices
                                                    )
                                                [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                    (
                                                        [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [3] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_documents
                                                    )
                                                [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                    (
                                                        [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [4] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_videos
                                                    )
                                                [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                    (
                                                        [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [5] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_images
                                                    )
                                                [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                    (
                                                        [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                    )
                                [File] => Array
                                    (
                                    )
                                [Price] => Array
                                    (
                                    )
                                [Stock] => Array
                                    (
                                    )
                                [TextLine] => Array
                                    (
                                        [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_short_description
                                                    )
                                                [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                    (
                                                        [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => 180x80x1,5cm
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [1] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_onrequest
                                                    )
                                                [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                    (
                                                        [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [2] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_topproduct
                                                    )
                                                [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                    (
                                                        [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [3] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_variant_attr_1
                                                    )
                                                [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                    (
                                                        [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [4] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_variant_attr_2
                                                    )
                                                [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                    (
                                                        [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [5] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_variant_master_sku
                                                    )
                                                [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                    (
                                                        [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => 
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                        [6] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                            (
                                                [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                    (
                                                        [value] => ses_weight
                                                    )
                                                [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                    (
                                                        [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                            (
                                                                [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                    (
                                                                        [value] => 
                                                                    )
                                                                [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                    (
                                                                        [translation] => Array
                                                                            (
                                                                                [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                    (
                                                                                        [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => ger-DE
                                                                                            )
                                                                                        [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                            (
                                                                                                [value] => 2
                                                                                            )
                                                                                    )
                                                                            )
                                                                    )
                                                            )
                                                        [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                            (
                                                                [value] => 
                                                            )
                                                    )
                                            )
                                    )
                                [TextBlock] => Array
                                    (
                                    )
                                [Int] => Array
                                    (
                                    )
                                [Float] => Array
                                    (
                                    )
                                [Bool] => Array
                                    (
                                    )
                            )
                    )
    ```

## How to add new fields to map project specific fields

Copy the file `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/mapping/wc3-nav/xsl/response.itemtransfer.xsl` in `ProjectBundleName/Resources/mapping/wc3-nav/xsl` and edit it.

To add for example, the field **VAT_Prod_Posting_Group** from the response:

``` 
Array
(
    [SoapResponse] => Array
        (
            [ItemTransferResult] => Array
                (
                    [Item] => Array
                        (
                            [0] => Array
                                (
                                    [No] => 0002
                                    [No_2] => 
                                    [Description] => Waage / Personenwaage GS 39, Glaswaage
                                    [Search_Description] => WAAGE / PERSONENWAAGE GS 39, GLASWAAGE
                                    [Description_2] => mit Sprachfunktion, 4 Speicher, 150kg
                                    [Base_Unit_of_Measure] => STK
                                    [Net_Weight] => 2.4
                                    [Freight_Type] => 
                                    [Country_Region_Purchased_Code] => 
                                    [VAT_Bus_Posting_Gr_Price] => 
                                    [Gen_Prod_Posting_Group] => 07-19
                                    [Country_Region_of_Origin_Code] => 
                                    [Tax_Group_Code] => 
                                    [VAT_Prod_Posting_Group] => 19 <-- field to add
```

Add the following block to the xsl file under the <Datamap> section:

``` xml
<Datamap>
...
 <TextLine>
    <identifier>ses_vat_prod_posting_group</identifier><!-- Identifier -->
    <TextField>
        <text>
            <encoding></encoding>
            <translations ses_unbounded="translation">
                <translation>
                    <language>ger-DE</language>
                    <value><xsl:value-of select="VAT_Prod_Posting_Group"/></value><!-- The name of the field from the response -->
                </translation>
            </translations>
        </text>
        <SesExtension />
    </TextField>
</TextLine>
```

To add a field of type array, like this one:

``` 
[Attributes] => Array
                                        (
                                            [0] => Array
                                                (
                                                    [Code] => ARTIKELART
                                                    [Label] => Artikelart
                                                    [Value] => Lagerartikel
                                                )

                                            [1] => Array
                                                (
                                                    [Code] => EINHEIT
                                                    [Label] => Einheit
                                                    [Value] => STK
                                                ) 
```

add an array block, also in the Datamap section:

``` xml
<Datamap>
...
<Array>
    <identifier>ses_attributes</identifier>
    <ArrayField>
        <array>
            <encoding></encoding>
            <translations ses_unbounded="translation">
                <translation ses_tree="value">
                    <language>ger-DE</language>
                    <value><xsl:copy-of select="Attributes" /></value>
                </translation>
            </translations>
        </array>
        <SesExtension />
    </ArrayField>
</Array>
```
