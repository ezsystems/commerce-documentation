# Reusable address template

## Goal

This template is used to display addresses in a more flexible way.

|                                                           |       |
| --------------------------------------------------------- | ------ |
| Template HTML | `\vendor\silversolutions\silver.e-shop\src\Silversolutions\Bundle\EshopBundle\Resources\views\parts\address.html.twig` |
| Template TXT  | `\vendor\silversolutions\silver.e-shop\src\Silversolutions\Bundle\EshopBundle\Resources\views\parts\address.txt.twig`  |

To override the template inside a project just create templates inside your project bundle that follow the same structure and have the same name

## Configuration

|Parameter|Description|
|--- |--- |
|address|address to be displayed. Must be type of Party object (eg. buyerAddress)|
|class|class used for styling (eg. styled_list)|
|displayEmail|if set to true, email address will be displayed in template. Default value is true.|
|displayPhone|if set to true,phone number will be displayed in template. Default value is true.|
|raw|if set to:</br>true - template will be displayed without formating</br>false - template will be displayed in unordered list</br>Default value is false.|


## Examples

``` html+twig
{{ include('SilversolutionsEshopBundle:parts:address.html.twig', {'class' : 'styled_list', 'address' : buyerAddress, 'displayEmail' : false, 'displayPhone' : false}) }}

{{ include('SilversolutionsEshopBundle:parts:address.html.twig', {'address' : address, 'displayEmail' : false, 'displayPhone' : false, 'raw' : true}) }}

{{ include('SilversolutionsEshopBundle:parts:address.txt.twig', {'address' : deliveryAddress, 'displayEmail' : false}) }}
```
