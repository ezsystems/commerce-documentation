#  TextBlockField 

`TextBlockField` is the representative implementation of `AbstractField` for a multi line text (or DOMDocument).

A new `TextBlockField` can be created using the following data:

``` 
use Silversolutions\Bundle\EshopBundle\Content\Fields\TextBlockField;

// Usage: 
$textBlockField = new TextBlockField(
    array(
        'text' => 'This is the <b>description</b> of the product'
    )
);
```

TextBlockField object can be <span lang="en">reliable serialized, such it implements the magic \_\_sleep() and \_\_wakeup() methods.
