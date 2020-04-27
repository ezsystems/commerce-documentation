# ValueObject

|           |                                              |
| --------- | -------------------------------------------- |
| Namespace | `Silversolutions\Bundle\EshopBundle\Content` |

The generic super class `ValueObject` is a convenient class which has magic `__get()` and `__set()` methods provided for inheriting classes. Every class, which inherits from this class, has the possibility to set all properties on construction. The properties are read-only accessible via the arrow operation afterwards. Of course you also can add standard set methods in your inheriting class for specific properties.

To validate the given properties easily, the optional property `$checkProperties` could be use by overwrite.

# Example 1

The following example creates a `Person` class which extends the `ValueObject`.

``` php
class Person extends ValueObject
{
    protected $fullName;
    protected $age;
    protected $female;
    protected $size;
}
```

Use the `Person` class and create an object. The constructor takes all necessary properties as an array:

``` php
$person = new Person(
    array(
        'fullName' => 'James Bond',
        'age' => 44,
        'female' => false,
        'size' => 1.85,
    )
);
 
/**
 * Now the properties in $person are already set!
 * Access these properties (read-only) by using the magic __get() method of ValueObject:
 */
echo 'Agent name: ' . $person->fullName . ' (Age: ' . $person->age . ')';
```

Let's improve the **`Person`** class with a simple validation:

``` php
class Person extends ValueObject
{
    protected $fullName;
    protected $age;
    protected $female;
    protected $size;
    
    /** @var array $checkProperties */
    protected $checkProperties = array(
        array('name' => 'fullName', 'mandatory' => true,    'type' => 'string'),
        array('name' => 'age',      'mandatory' => true,    'type' => 'integer'),
        array('name' => 'female',   'mandatory' => true,    'type' => 'boolean'),
        array('name' => 'size',     'mandatory' => false,   'type' => 'float'),
    );
}
```

Now an exception is thrown, if you try to omit one of the `mandatory` properties, or if one of the given properties have not the valid variable type.

``` php
// results in InvalidArgumentException: 'Mandatory property "female" not given in $properties (class: "Person")':
$person = new Person(
    array(
        'fullName'  => 'James Bond',
        'age'       => 44,
    )
);
 
// results in InvalidArgumentException: 'Property "age" is not integer (class: "Person")':
$person = new Person(
    array(
        'fullName'  => 'James Bond',
        'age'       => '44',
        'female'    => false,
    )
);
```

Now let's append a "real" and more specific validation by extending the `validateProperties()` method.

``` php
class Person extends ValueObject
{
    protected $fullName;
    protected $age;
    protected $female;
    protected $size;
    
    /** @var array $checkProperties */
    protected $checkProperties = array(
        array('name' => 'fullName', 'mandatory' => true, 'type' => 'string'),
        array('name' => 'age',      'mandatory' => true, 'type' => 'integer'),
        array('name' => 'female',   'mandatory' => true, 'type' => 'boolean'),
        array('name' => 'size',     'mandatory' => false, 'type' => 'float'),
    );
    
    /**
     * {@inheritdoc}
     */
    protected function validateProperties(array $properties = array())
    {
        // use "simple" $checkProperties validation from ValueObject:
        parent::validateProperties($properties);
        
        /*
         * Property "size" is definitive a float (as checked in parent).
         * Check, if the size is between 0m and 2.5m
         */
        if ($properties['size'] < 0.0 || $properties['size'] > 2.5) {
            throw new \InvalidArgumentException('The given "size" is too '
                . 'small or too large!'
            );
        }
    }
}
```

# Example 2

Beside the "simple" data types (like int or array) you can also use a custom type.

Lets assume, your class needs have an attribute $request and we have to make sure it is an instance of **\\Symfony\\Component\\HttpFoundation\\Request**. You can do this like this:

``` php
class Example extends ValueObject
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

Please note the values of `type` and `isObject`. Only if `isObject` is true, we check if the attribute is instanceof `type`.
