# getBasket 

basket.get\_basket

method: getBasket

operation: basket

description: Gets the current basket

## Example

``` 
use Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\InputValueObjects\GetBasket as InputGetBasket;
use Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\OutputValueObjects\GetBasket as OutputGetBasket;

/** @var InputGetBasket $inputGetBasket */
$inputGetBasket = new InputGetBasket(array('request' => $request));

/** @var OutputGetBasket $outputGetBasket */
$outputGetBasket = $this->getBusinessApi()->call('basket.get_basket', $inputGetBasket);

$message = $this->getBasketMessage($outputGetBasket->basket);
```

## Input Parameters

``` 
namespace Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\InputValueObjects;

use Silversolutions\Bundle\EshopBundle\Content\ValueObject;

/**
 * Class GetBasket
 *
 * This class is used as an input parameter for an appropriate business method
 *
 * @property-read \Symfony\Component\HttpFoundation\Request $request
 */
class GetBasket extends ValueObject
{
    /** @var \Symfony\Component\HttpFoundation\Request $request */
    protected $request;
    /** @var array $checkProperties */
    protected $checkProperties = array(
        array(
            'name' => 'request',
            'mandatory' => true,
            'type' => '\Symfony\Component\HttpFoundation\Request',
            'isObject' => true
        )
    );
}
```

## `Returns Output`

``` 
namespace Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\OutputValueObjects;

use Silversolutions\Bundle\EshopBundle\Content\ValueObject;

/**
 * Class GetBasket
 *
 * This class is used as an output parameter for an appropriate business method
 *
 * @property-read \Silversolutions\Bundle\EshopBundle\Entity\Basket $basket
 */
class GetBasket extends ValueObject
{
    /** @var \Silversolutions\Bundle\EshopBundle\Entity\Basket $basket */
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