# ProductSelection 

## General

This Fieldtype offers a relation feature for products. It can be used with product stored in eZ or in eContent.

## Usage

Add a new field (Product relationlist) to a contenttype:

![](attachments/29818961/29821512.png)

In edit mode a editor can search for a product or sku and add product to the relation list:

  - The optional attribute "note" can be used e.g. to display a hint on the frontend (e.g. Offer). 

![](attachments/29818961/29821506.png)
## For developers

You can access the relation list data in twig using a limited list of attributes:

  - sku
  - name
  - note
  - image

**Example**:

```
<table class="table table-striped">
    {% for product in field.value.product_list %}
        <tr>
            <td>
                {% if product.image|default('') != '' %}
                   <img src="{{ product.image }}" height="40px" />
                {% endif %}
            </td>
            <td>{{ product.name }}</td>
        </tr>
    {% endfor %}
</table>
```

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-7-23\_14-11-58.png](attachments/29818961/29821509.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-23\_14-20-43.png](attachments/29818961/29821510.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2019-02-13 um 15.18.19.png](attachments/29818961/29821511.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-12-01 um 14.23.46.png](attachments/29818961/29821513.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-12-01 um 14.23.30.png](attachments/29818961/29821514.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-12-01 um 14.24.27.png](attachments/29818961/29821515.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2019-8-20\_9-49-15.png](attachments/29818961/29821512.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2019-8-20\_10-2-18.png](attachments/29818961/29821516.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2019-8-20\_10-11-13.png](attachments/29818961/29821506.png) (image/png)  
