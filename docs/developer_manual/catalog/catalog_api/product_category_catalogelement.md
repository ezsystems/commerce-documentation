#  Product category (CatalogElement) 

The CatalogElement class defines the generic product and category model which is used within eZ Commerce. It inherits from the general class [ValueObject](ValueObject_23560594.html) which offers a convenient way of setting properties of instances via the constructor and makes these properties public readable (like $valueObject-\>name).

Inheriting from CatalogElement there are some sub classes worth mentioning:

  - CatalogElementContainer
      - CatalogNode
  - ProductNode
      - OrderableProductNode
      - ProductNodeContainer
          - GroupedProductNode
          - ComplexProductNode
  - ProductType

**Predefined properties for `CatalogElement`**

Each `CatalogElement` has predefined properties. These methods are validated automated on constructor by the `validateProperties()` method.

<table>
<tbody>
<tr>
<td><strong>Identifier</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr>
<td><code>name</code></td>
<td><code>string</code></td>
<td>The name of the catalog</td>
</tr>
<tr>
<td><code>text</code></td>
<td><code>string</code></td>
<td>A short introduction text for the catalog</td>
</tr>
<tr>
<td><code>image</code></td>
<td><code>ImageField (FieldInterface)</code></td>
<td>An image for the catalog</td>
</tr>
<tr>
<td><code>path</code></td>
<td><code>array</code></td>
<td>The path of the catalog (array of identifier)</td>
</tr>
<tr>
<td><code>url</code></td>
<td><code>string</code></td>
<td>The internal URL of the catalog. This url should not be used for generating a link! Please use <code>seoUrl</code> instead</td>
</tr>
<tr>
<td><code>seoUrl</code></td>
<td><code>string</code></td>
<td>The human readable URL of the category</td>
</tr>
<tr>
<td><code>permanentUrl</code></td>
<td><code>string</code></td>
<td><p>The internal permanent URL</p></td>
</tr>
<tr>
<td><code>parentElementIdentifier</code></td>
<td><code>string</code></td>
<td>The unqique identifier of the parent catalog</td>
</tr>
<tr>
<td><code>identifier</code></td>
<td><code>string</code></td>
<td>The unqique identifier</td>
</tr>
<tr>
<td><code>dataMap</code></td>
<td><pre><code>FieldInterface[]</code></pre></td>
<td>A list of Field objects</td>
</tr>
<tr>
<td><pre><code>cacheIdentifier</code></pre></td>
<td><pre><code>int|string</code></pre></td>
<td>cache identifier of element to use as key in cache storage</td>
</tr>
</tbody>
</table>

There are 4 public methods to set properties: 

  - setImage, 
  - setName, 
  - setText, 
  - setCacheIdentifier

**Validators for `CatalogElement`**

List of possible validators to used when attributes are set in CatalogElement

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Parameters</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>validateStringAttribute</code></pre></td>
<td><pre><code>$value,</code></pre>
<pre><code>$attribute</code></pre></td>
<td>checks if the value is a valid string</td>
</tr>
<tr>
<td><pre><code>validateBooleanAttribute</code></pre></td>
<td><pre><code>$value,</code></pre>
<pre><code>$attribute</code></pre></td>
<td>checks if the value is a valid boolean</td>
</tr>
<tr>
<td><pre><code>validateFloatAttribute</code></pre></td>
<td><pre><code>$value,</code></pre>
<pre><code>$attribute</code></pre></td>
<td>checks if the value is a valid float</td>
</tr>
<tr>
<td><pre><code>validateIntegerAttribute</code></pre></td>
<td><pre><code>$value,</code></pre>
<pre><code>$attribute</code></pre></td>
<td>checks if the value is a valid integer</td>
</tr>
<tr>
<td><pre><code>validateFieldAttribute</code></pre></td>
<td><pre><code>$value,</code></pre>
<pre><code>$attribute,</code></pre>
<pre><code>$fieldType</code></pre></td>
<td>checks if the value is of given Field Type</td>
</tr>
<tr>
<td><pre><code>validateArrayAttribute</code></pre></td>
<td><pre><code>$value,</code></pre>
<pre><code>$attribute</code></pre></td>
<td>checks if the value is an array</td>
</tr>
<tr>
<td><pre><code>validateArrayOfAttribute</code></pre></td>
<td><pre><code>$value,</code></pre>
<pre><code>$attribute,</code></pre>
<pre><code>$class</code></pre></td>
<td>checks if the value is an array of concrete class (interface)</td>
</tr>
</tbody>
</table>

**  
**

Concrete implementations of the `CatalogElement` class requires you to extend `validateProperties()`. This method is used to validate all given properties in the constructor of the class. As you do not want to overwrite the given implementation but extend it, you want to use `parent::validateProperties($properties)` in your implementation.

**Example**

**Example: Extending validateProperties()** Expand source 

``` 
/**
 * Simple class which extends OrderableProductNode (CatalogElement)
 */ 
class MyOrderableProductNode extends OrderableProductNode
{
    /**
     * An additional instance of a Field
     * @var FieldInterface
     */
    protected $myField;
    
    protected function validateProperties(array $properties = array())
    {
        // call validateProperties() from super class to validate default properties
        parent::validateProperties($properties);
        
        // ... now validate my specialized property "myField"
        if (
            isset($properties['myField'])
            && !($properties['myField'] instanceof FieldInterface)
        ) {
            $message = 'Attribute "myField" has wrong data type: '
                    . gettype($value)
                    . '. Instance of class FieldInterface expected.';
            throw new \InvalidArgumentException($message);
        }
    }
}
```

## Class diagram

![](attachments/23560458/23563414.png)

## Sub classes

  - [CatalogElementContainer & CatalogNode](/pages/createpage.action?spaceKey=EZC14&title=CatalogElementContainer+%26+CatalogNode&linkCreation=true&fromPageId=23560458)
  - [ProductNode & OrderableProductNode](23560315.html)
  - [ProductType](ProductType_23560981.html)

## Attachments:

![](images/icons/bullet_blue.gif) [uml\_v0.2.1.png](attachments/23560458/23563413.png) (image/png)  
![](images/icons/bullet_blue.gif) [uml\_v0.2.1.png](attachments/23560458/23563414.png) (image/png)  
