# loadProducts 

catalog.load\_products

method: loadProducts

operation: catalog

description: Loads products from storage

## Example call to Business API

**Call Catalog Business API**

``` 
       /** @var InputLoadList $input */
        $input = new InputLoadList(
            array(
                'locationId' => 136,
                'limit' => 3,
                'offset' => 3,
                'language' => 'de_DE',
                'filterType' => 'productList',
            )
        );

        /** @var OutputLoadList $output */
        $output = $this->getBusinessApi()->call('catalog.load_products', $input);

        $html = $this->renderView(
            'SilversolutionsEshopBundle:Catalog:listProductNodes.html.twig',
            array(
                'catalogList' => $output->catalogList,
                'params' => $data,
                'locationId' => $output->locationId,
            )
        );
```

## BusinessAPI loadProducts

Business API calls catalog service to fetch all products and passes the filterType as an argument.

``` 
public function loadProducts(InputLoadList $input)
    {
        list($catalogList, $catalogCount) = $this->catalogService->fetchChildrenList(
            $input->locationId,
            self::DEFAULT_FETCH_DEPTH,
            array('filterType' => $input->filterType),
            $input->language,
            $input->offset,
            $input->limit
        );
        /** @var OutputLoadList $output */
        $output = new OutputLoadList(
            array(
                'catalogList' => $catalogList,
                'catalogCount' => $catalogCount,
                'locationId' => $input->locationId
            )
        );
        return $output;
    }
```

## CatalogService

It provides a method for searching catalog elements using the proper data provider. It also gets prices and total count of elements without offset, limit.

**fetchChildrenList** returns an **array** with 2 elements:

\- **$catalogList** - list of catalog elements  
\- **$catalogCount** - total number of elements (without offset and limit) for pagination purpouse

``` 
public function fetchChildrenList($identifier, $depth, $filter, $languages = null, $offset = 0, $limit = 3)
    {
        /** @var CatalogListResult $catalogList */
        $catalogList = $this->dataProviderService->getDataProvider()
            ->fetchChildrenList($identifier, $depth, $filter, $languages, $offset, $limit);
        if ($catalogList->countChildren() > 0) {
            // handle prices
            /** @var $priceService \Siso\Bundle\PriceBundle\Service\PriceService */
            $priceRequest = PriceRequest::getPriceRequestByList(
                $catalogList,
                array(),
                array('customerNumber' => $this->customerService->getCurrentCustomer()->getCustomerNumber(),
                    'source' => PriceService::SOURCE_CATALOG,
                )
            );
            if ($priceRequest->getRequestProductCount() > 0) {
                $this->priceService->getPriceProvider()->requestPrice($priceRequest);
            }
        }
        $catalogCount = $this->dataProviderService->getDataProvider()
            ->countChildrenList($identifier, $depth, $filter, $languages);
        return array($catalogList, $catalogCount);
    }
```

## Configuration

The filter *catalogList* and *productList* are defined in the configuration file. It is used in catalog data provider to filter and sort all elements or products list from catalog

**silver.eshop.yml**

``` 
silver_eshop.default.ez5_catalog_data_provider.filter:
        navigation:
           ...
        catalogList:
            contentTypes: ["ses_category", "ses_product"]
            sortClauses:
                -
                    clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                    order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
        productList:
            contentTypes: ["ses_product"]
            sortClauses:
                -
                    clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                    order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
```

## Input Parameters

``` 
namespace Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\InputValueObjects;

use Silversolutions\Bundle\EshopBundle\Content\ValueObject;

/**
 * Class LoadList
 *
 * This class is used as an input parameter for an appropriate business method
 *
 * @property-read int $locationId
 * @property-read int $limit
 * @property-read int $offset
 * @property-read string $language
 */
class LoadList extends ValueObject
{
    /**
     * @var int $locationId
     */
    protected $locationId;
    /**
     * @var int $limit
     */
    protected $limit;
    /**
     * @var int $offset
     */
    protected $offset;
    /**
     * @var string $language
     */
    protected $language;
    /**
     * @var string $filterType
     */
    protected $filterType;
    /**
     * @var array $checkProperties
     */
    protected $checkProperties = array(
        array('name' => 'locationId', 'mandatory' => true, 'type' => 'int'),
        array('name' => 'limit', 'mandatory' => true, 'type' => 'int'),
        array('name' => 'offset', 'mandatory' => true, 'type' => 'int'),
        array('name' => 'language', 'mandatory' => false, 'type' => 'string'),
        array('name' => 'filterType', 'mandatory' => false, 'type' => 'string'),
    );
}
```

## Returns Output

``` 
namespace Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\OutputValueObjects;
use Silversolutions\Bundle\EshopBundle\Catalog\CatalogListResult;
use Silversolutions\Bundle\EshopBundle\Content\ValueObject;
/**
 * Class LoadList
 *
 * This class is used as an output parameter for an appropriate business method
 *
 * @property-read int $locationId
 * @property-read CatalogListResult $catalogList
 * @property-read int $catalogCount
 */
class LoadList extends ValueObject
{
    /** @var int $locationId */
    protected $locationId;
    /** @var CatalogListResult $catalogList */
    protected $catalogList;
    /** @var int $catalogCount */
    protected $catalogCount;
    /** @var array $checkProperties */
    protected $checkProperties = array(
        array('name' => 'locationId', 'mandatory' => true, 'type' => 'int'),
        array(
            'name' => 'catalogList',
            'mandatory' => true,
            'type' => 'Silversolutions\Bundle\EshopBundle\Catalog\CatalogListResult',
            'isObject' => true,
        ),
        array('name' => 'catalogCount', 'mandatory' => true, 'type' => 'int'),
    );
}
```
