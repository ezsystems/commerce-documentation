# Doctrine Field Type mapping

eZ Commerce uses Doctrine as ORM for most of the cases when accessing the database.
In some Doctrine entities, like [Basket or BasketLine](../guide/basket/basket_api/basket_data_model.md)
it uses objects or arrays to handle the attributes.

By default, Doctrine uses the `serialize()` method when storing this data in the database.

If the data cannot be unserialized (for example maybe because the object changed in the meantime, or the data is corrupted),
Doctrine throws an exception. In that case the whole basket or basket line cannot be returned.

That is why eZ Commerce overrides the Doctrine mapping types for arrays and objects so the exception can be handled.
In that case only the corrupted attribute is empty. For example, you can have a basket line, but the data map would be empty if it cannot be unserialized.

### Configuration

``` yaml
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
