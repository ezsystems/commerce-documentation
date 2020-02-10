#  Reusable address template 

## Goal

This template is used to display addresses in a more flexible way.

|                                                           |                                                                                                                                 |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **Template HTML** | \\vendor\\silversolutions\\silver.e-shop\\src\\Silversolutions\\Bundle\\EshopBundle\\Resources\\views\\parts\\address.html.twig |
| **Template TXT**                                          | \\vendor\\silversolutions\\silver.e-shop\\src\\Silversolutions\\Bundle\\EshopBundle\\Resources\\views\\parts\\address.txt.twig  |

To override the template inside a project just create templates inside your project bundle that follow the same structure and have the same name

## Configuration

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>address</code></pre></td>
<td>address to be displayed. Must be type of Party object (eg. buyerAddress)</td>
</tr>
<tr>
<td><pre><code>class </code></pre></td>
<td>class used for styling (eg. styled_list)</td>
</tr>
<tr>
<td><pre><code>displayEmail</code></pre></td>
<td>if set to true, email address will be displayed in template. Default value is true.</td>
</tr>
<tr>
<td><pre><code>displayPhone</code></pre></td>
<td>if set to true,phone number will be displayed in template. Default value is true.</td>
</tr>
<tr>
<td><pre><code>raw</code></pre></td>
<td><p>if set to:</p>
<ul>
<li>true - template will be displayed without formating</li>
<li>false - template will be displayed in unordered list</li>
</ul>
<p>Default value is false.</p></td>
</tr>
</tbody>
</table>

## Examples

``` 
{{ include('SilversolutionsEshopBundle:parts:address.html.twig', {'class' : 'styled_list', 'address' : buyerAddress, 'displayEmail' : false, 'displayPhone' : false}) }}

{{ include('SilversolutionsEshopBundle:parts:address.html.twig', {'address' : address, 'displayEmail' : false, 'displayPhone' : false, 'raw' : true}) }}

{{ include('SilversolutionsEshopBundle:parts:address.txt.twig', {'address' : deliveryAddress, 'displayEmail' : false}) }}
```
