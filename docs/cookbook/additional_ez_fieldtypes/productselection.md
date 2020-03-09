# ProductSelection

## General

This Fieldtype offers a relation feature for products. It can be used with product stored in eZ or in eContent.

## Usage

Add a new field (Product relationlist) to a contenttype:

![](../img/additional_ez_fieldtypes_1.png)

In edit mode a editor can search for a product or sku and add product to the relation list:

- The optional attribute "note" can be used e.g. to display a hint on the frontend (e.g. Offer).

![](../img/additional_ez_fieldtypes_2.png)

## For developers

You can access the relation list data in twig using a limited list of attributes:

- sku
- name
- note
- image

**Example**:

``` html+twig
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
