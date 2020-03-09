# Doctrine Mapping Datatypes

eZ Commerce uses Doctrine as ORM for most of the cases when accessing the DB data. In some doctrine entities, like [Basket or BasketLine](Basket-data-model_23560234.html) we are using objects or arrays to handle the attributes.

By default, doctrine use the serialize() method when storing these data to the DB.

## Issue

If the data can not be unserialized for some reason (maybe because the object changed in the meantime, or the data is corrupted), doctrine will throw an exception. In that case the whole basket, or basket line can not be returned.

## Solution

For that reason eZ Commerce overrides the Doctrine Mapping Types for Arrays and Objects so the exception can be handled. In that case only the corrupted attribute will be empty, eg. we will have basket line, but the datamap would be empty if can not be unserialized.

### Configuration

``` yaml
# Doctrine Configuration
doctrine:
    dbal:
        types:
            object: Silversolutions\Bundle\EshopBundle\Model\Doctrine\ObjectType
            array: Silversolutions\Bundle\EshopBundle\Model\Doctrine\ArrayType
```

### ArrayType

``` php
namespace Silversolutions\Bundle\EshopBundle\Model\Doctrine;

use Doctrine\DBAL\Platforms\AbstractPlatform;
use Doctrine\DBAL\Types\ArrayType as BaseType;

class ArrayType extends BaseType
{
    /**
     * Overrides parent to be able to handle unserialize exception
     *
     * @param mixed $value
     * @param AbstractPlatform $platform
     * @return array|null
     *
     */
    public function convertToPHPValue($value, AbstractPlatform $platform)
    {
        if ($value === null) {
            return null;
        }

        $value = (is_resource($value)) ? stream_get_contents($value) : $value;
        try {
            $val = unserialize($value);
            if ($val === false && $value != 'b:0;') {
                return null;
            }

            return $val;
        } catch(\Exception $e) {
            return null;
        }
    }
}
```

### ObjectType

``` php
namespace Silversolutions\Bundle\EshopBundle\Model\Doctrine;

use Doctrine\DBAL\Types\ObjectType as BaseType;
use Doctrine\DBAL\Platforms\AbstractPlatform;

class ObjectType extends BaseType
{
    /**
     * Overrides parent to be able to handle unserialize exception
     *
     * @param mixed $value
     * @param AbstractPlatform $platform
     * @return object|null
     *
     */
    public function convertToPHPValue($value, AbstractPlatform $platform)
    {
        if ($value === null) {
            return null;
        }

        $value = (is_resource($value)) ? stream_get_contents($value) : $value;
        try {
            $val = unserialize($value);
            if ($val === false && $value !== 'b:0;') {
                return null;
            }

            return $val;
        } catch(\Exception $e) {
            return null;
        }
    }
}
```
