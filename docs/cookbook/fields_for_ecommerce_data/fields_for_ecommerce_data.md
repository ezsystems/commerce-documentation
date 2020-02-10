#  Fields for ecommerce data 

eZ Commerce uses own fields to store ecommerce related data, e.g for the catalog or basket.

The shop provides a flexible way to store data using concrete instances of classes implementing the **`FieldInterface`** and inheriting from the **`AbstractField`** class. Fields are used for fixed attributes of a product/catalog and flexible attributes (property "`dataMap`" in [`CatalogElement`](23560458.html)).

Each instance of a concrete Field will provide the following methods:

| Method                | Description                                                         |
| --------------------- | ------------------------------------------------------------------- |
| `getTypeIdentifier()` | Returns the identifier of the Field (e.g. "sesimage")               |
| `isSearchable()`      | Returns true, if the Field is searchable                            |
| `isEmptyValue()`      | Returns true, if the value of the Field is empty                    |
| `getEmptyValue()`     | Returns an empty version of the Field                               |
| `toHash()`            | Returns a associative array (hash) from the Field                   |
| `fromHash($hash)`     | Returns a created instance of the Field by associative array (hash) |
| `toString()`          | Returns the value of the Field as string                            |

All AbstractField objects can be serialized, if you are using the methods toHash() and fromHash() before/after (un)serialize method.

**Example:**

``` 
//serialize
if($field instanceof AbstractField) {
    $fieldValue = serialize($field->toHash());
}
```

## Class diagram

![](attachments/23560470/23563415.png)

## Implemented concrete Field classes

| Type             | Used for                                                     | identifier     |
| ---------------- | ------------------------------------------------------------ | -------------- |
| `TextLineField`  | A simple String without HTML-Code                            | `sestextline`  |
| `TextBlockField` | A rich text field containing HTML code                       | `sestextblock` |
| `ImageField`     | An image contains a path to an image and an alternative text | `sesimage`     |
| `ArrayField`     | A structured array                                           | `sesarray`     |
| `PriceField`     | An instance of the Price class                               | `sesprice`     |

## Used templates for rendering

For each concrete `Field` a templates has to be provided in order to render the `Field` in the template. 

The templates have to be provided in the folder `FieldTypes`. The name of the template has to start with the identifier of the `Field`, e.g.

  - `ImageField.html.twig`
  - `TextBlockField.html.twig`
  - `TextLineField.html.twig`
  - `PriceField.html.twig`

The renderer provides a parameter `$field` providing the object of the given field.

Call from a twig template, via ses\_render\_field:

``` 
{{ ses_render_field(catalogElement, 'longDescription')|raw }}
```

## TextLineField

`TextLineField` is the representative implementation of `AbstractField` for a single line of text.

A new `TextLineField` can be created using the following data:

``` 
use Silversolutions\Bundle\EshopBundle\Content\Fields\TextLineField;
 
// Usage: 
$textLineField = new TextLineField(array('text' => 'This is the name of the product'));
```

## TextBlockField

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

## ImageField

`ImageField` is the representative implementation of `AbstractField` for an image. An image is identified and initiated by a given path and an optional alternative text.

A new `ImageField` can be created using the following data:

``` 
use Silversolutions\Bundle\EshopBundle\Content\Fields\ImageField;

// Usage:
$imagePath = 'var/storage/images/product_image.jpg';
$imageField = new ImageField(
    array(
        'alternativeText' => 'a nice product',
        'fileName'        => basename($imagePath),
        'fileSize'        => filesize($imagePath),
        'path'            => $imagePath,
    )
);
```

## ArrayField

`ArrayField` is the representative implementation of `AbstractField` for a structured array.

A new `ArrayField` can be created using the following data:

``` 
use Silversolutions\Bundle\EshopBundle\Content\Fields\ImageField;

// Usage:
$myArray = array (
    'weight' => '10 kg',
    'color' => 'red'
);
$arrayField = new ArrayField(array('array' =>  $myArray));
```

## PriceField

`PriceField` is the representative implementation of `AbstractField` for a **`Price`**.

A new `PriceField` can be created using the following data:

``` 
use Silversolutions\Bundle\EshopBundle\Content\Fields\PriceField;
use Siso\Bundle\PriceBundle\Model\Price;
 
// Usage: 
$price = new Price(
    array(
        'price' => 99.99,
        'isVatPrice' => true,
        'vatCode' => 19.0,
        'currency' => 'EUR',
        'source' => 'ERP',
    )
);
$priceField = new PriceField(array('price' => $price));
```

#### Rendering

Please refer to [Rendering for prices](https://doc.silver-eshop.de/display/EZC14/Rendering+for+prices) to see the possibilities of outputting a PriceField using the `ses_render_field()` function.

It is also possible to render a priceField with a twig function **ses\_render\_price().** The difference see in the table below:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Twig function</th>
<th>Paramaters</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>ses_render_field()</code></td>
<td><p>$catalogElement</p>
<p>string $fieldIdentifier</p>
<p>array $<a href="https://doc.silver-eshop.de/display/EZC14/Rendering+for+prices">params</a></p></td>
<td><p>more general -</p>
<p>renders also other FieldInterface $fields from $catalogElement</p>
<p>like TextBlockField, ImageField, PriceField</p></td>
</tr>
<tr>
<td><strong><code>ses_render_price()</code></strong></td>
<td><p>$catalogElement</p>
<p>PriceField $priceField</p>
<p>array $<a href="https://doc.silver-eshop.de/display/EZC14/Rendering+for+prices">params</a></p></td>
<td><p>renders only PriceField $price</p>
<p> </p></td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [field\_uml\_v.0.1.png](attachments/23560470/23563415.png) (image/png)  
