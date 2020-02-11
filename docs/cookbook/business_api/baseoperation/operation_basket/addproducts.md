#  addProducts 

basket.add\_products

method: addProducts

operation: basket

description: Adds one or more products to the basket

exceptions

    \InvalidArgumentException 

\- thrown if variant without variantCode is choosen

## Example

``` 
                 $outputGetBasket = $this->getBusinessApi()->call('basket.get_basket', $inputGetBasket);
                //clear all messages for each request
                $outputGetBasket->basket->clearAllMessages();
                $itemData = new ItemData(
                    array(
                        'quantity'    => 1,
                        'isVariant'   => false,
                        'variantCode' =>  '',
                        'sku'         => '1000',
                    )
                );
                /** @var InputAddItemToBasket $inputAddItemToBasket */
                $inputAddItemToBasket = new InputAddItemToBasket(
                   array(
                       'itemData' => $itemData,
                       'basket'   => $outputGetBasket->basket,
                   )
                );
                try {
                    $outputGetBasket = $this->getBusinessApi()->call('basket.add_products', $inputAddItemToBasket);
                } catch (\InvalidArgumentException $e) {
                    // ....
                }
                $message = $this->getBasketMessage($outputGetBasket->basket);
    
```

## Input parameters

``` 
namespace Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\InputValueObjects;
 
class AddItemToBasket extends ValueObject
{
 
    /**
     * @var \Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\InputValueObjects\AddItemToBasket\ItemData
     */
    protected $itemData;
    /**
     * @var \Silversolutions\Bundle\EshopBundle\Entity\Basket $basket
     */
    protected $basket;
    /**
     * @var array
     */
    protected $checkProperties = array(
        array(
            'name' => 'itemData',
            'mandatory' => true,
            'type' => '\Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\InputValueObjects\AddItemToBasket\ItemData',
            'isObject' => true,
        ),
        array(
            'name' => 'basket',
            'mandatory' => true,
            'type' => '\Silversolutions\Bundle\EshopBundle\Entity\Basket',
            'isObject' => true
        )
}
```

``` 
namespace Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\InputValueObjects\AddItemToBasket;

use Silversolutions\Bundle\EshopBundle\Content\ValueObject;

/**
 * This class bundles all data, which is necessary for a basket item line.
 *
 * @property-read float $quantity
 * @property-read string $isVariant
 * @property-read string $variantCode
 * @property-read string $sku
 */
class ItemData extends ValueObject
{
    /**
     * @var string
     */
    protected $quantity;
    /**
     * @var string
     */
    protected $isVariant;
    /**
     * @var string
     */
    protected $variantCode;
    /**
     * @var string
     */
    protected $sku;
    /**
     * @var array
     */
    protected $checkProperties = array(
        array('name' => 'quantity', 'mandatory' => true, 'type' => 'string'),
        array('name' => 'isVariant', 'mandatory' => false, 'type' => 'string'),
        array('name' => 'variantCode', 'mandatory' => false, 'type' => 'string'),
        array('name' => 'sku', 'mandatory' => true, 'type' => 'string'),
    );
}
```

## Returns Output

``` 
namespace Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\OutputValueObjects;
use Silversolutions\Bundle\EshopBundle\Content\ValueObject;
/**
 * Class AddItemToBasket
 *
 * This class is used as an output parameter for an appropriate business method
 *
 * @property-read \Silversolutions\Bundle\EshopBundle\Entity\Basket $basket
 */
class AddItemToBasket extends ValueObject
{
    /**
     * @var \Silversolutions\Bundle\EshopBundle\Entity\Basket $basket
     */
    protected $basket;
    /** @var array $checkProperties */
    protected $checkProperties = array(
        array(
            'name' => 'basket',
            'mandatory' => true,
            'type' => '\Silversolutions\Bundle\EshopBundle\Entity\Basket',
            'isObject' => true
        )
    );
}
```
