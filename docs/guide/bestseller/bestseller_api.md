# Bestseller API

## Services

### `BestsellerService`

`vendor/silversolutions/silver.e-shop/src/Siso/Bundle/SearchBundle/Service/BestsellerService.php`
  
Fetches bestsellers from Solr.
    
## `BasketLineSumDeterminationService`

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Service/Bestsellers/BsasketLineSumDeterminationService.php`
  
Returns an array with every SKU from confirmed baskets and a sum of all corresponding basket lines found.

## Indexer Plugins

## `EzBestsellerIndexerPlugin`

`vendor/silversolutions/silver.e-shop/src/Siso/Bundle/SearchBundle/Service/EzBestsellerIndexerPlugin.php`
   
Adds an additional Solr field with a sum of basket lines.
    
## `EcontentBestsellerIndexerPlugin`

`vendor/silversolutions/silver.e-shop/src/Siso/Bundle/SearchBundle/Service/EcontentBestsellerIndexerPlugin.php`
  
Adds an additional Solr field with a sum of basket lines.
