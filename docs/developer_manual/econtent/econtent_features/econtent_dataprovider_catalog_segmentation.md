#  Econtent dataprovider - catalog segmentation 

Especially in B2B shops catalogs are often segmented even by customers. 

  - An ERP system might provide a segmentation code such as "certified" and this customer should get more products than a anonymous user or a customer having a different code
  - Often customer even have individual products which are available for this customer only

A standard shop often is not able to implement this feature. 

## How does it work

econtent is able to use one more database table which can be used to segment the catalog. 

``` 
desc sve_object_catalog;
+--------------+------------------+------+-----+---------+-------+
| Field        | Type             | Null | Key | Default | Extra |
+--------------+------------------+------+-----+---------+-------+
| node_id      | int(11) unsigned | NO   | PRI | NULL    |       |
| catalog_code | char(20)         | NO   | PRI |         |       |
+--------------+------------------+------+-----+---------+-------+
```

Foreach node\_id one or more catalog\_codes can be defined. 

The standard supports a by siteaccess or siteaccess group defined setup. This can be extended in the project by extending the econtent service and providing a own logic for providing the catalogcodes .e.g on customer base. Details see 

## How to implement different catalog filtering

Let's assume that in the project which uses econtent, catalog elements have assigned roles (so different users can see different content). To do this you need to adjust econtent. 

Steps:

1.  Adjust configuration like example below:

    ``` 
    silver_econtent.default.filter_SQL_where: 'sve_object_catalog.node_id = obj.node_id AND sve_object_catalog.catalog_code IN (%%catalog_code%%)'
    
    silver_econtent.default.catalog_filter_default_catalogcode:
        - ANONYMOUS

    silver_econtent.default.section_filter: enabled
    ```

    Please note that in addition to 'default' the following configuration exists because admin does not use normally segmentation although if you enable segmentation for 'default':
    
        silver_econtent.admin.section_filter: disabled

## Custom logic for segmentation

If you need custom logic please override the service 'ConfigEcontentCatalogSegmentationService' in order to return the catalogcodes which shall be used in your context (e.g. depending on the user) . 

Catalog Segmentation uses a service to define which users are allowed to see the content specified in catalog segmentation.

The default configuration uses a very simple service:

    ConfigEcontentCatalogSegmentationService

that returns a collection of catalog codes that is specified in a configuration as described above. The service implements the `EcontentCatalogSegmentationServiceInterface`

The purpose of the interface is to be reimplemented in projects with a custom algorithm that determines the respective  to that should be used to search and to show specific products to specific users.

Example of a new service that replaces the default implementation.

``` 
<?php
/**
 * Product silver.e-shop
 *
 * A powerful e-commerce solution for B2B online shops / portals and complex
 * online applications that have access to ERP data, usually in real time.
 * http://www.silversolutions.de/eng/Products/silver.e-shop
 *
 * This file contains the class MyCustomUserEcontentCatalogSegmentationService
 *
 * @copyright Copyright (C) 2013 silver.solutions GmbH. All rights reserved.
 * @license see vendor/silversolutions/silver.e-shop/license_txt_ger.pdf
 * @version $Version$
 * @package silver.e-shop
 */
namespace Silversolutions\Bundle\EshopBundle\Service;

use eZ\Publish\Core\MVC\ConfigResolverInterface;
use Silversolutions\Bundle\EshopBundle\Api\EcontentCatalogSegmentationServiceInterface;

/**
 * Simple user group based implementation.
 */
class MyCustomUserEcontentCatalogSegmentationService implements EcontentCatalogSegmentationServiceInterface
{
    /**
     * @var string
     */
    private $userGroup;

    /**
     * Set via external instance depending to request session
     *
     * @param string $groupCode
     */
    public function setUserGroup($groupCode)
    {
        $this->userGroup = $groupCode;
    }

    /**
     * The logic to fetch the desired catalog codes should be placed here:
     */
    public function getCatalogCodes()
    {
        // Get catalog codes from Ez users or from ERP (static array here)
        if ($this->userGroup === 'VIP') {
            return array('VIP', 'NORMAL')
        } else {
            return array('NORMAL');
        }
    }
}
```

## How the catalog codes are indexed

The Econtent indexer indexes all catalog codes from an element in a multi-value field called: 

    main_catalog_segments_ms

This field will store all catalog codes for a given element. 

``` 
# Example of Solr index
"main_catalog_segments_ms": [
    "ALL",
    "NORMAL"
]
```
