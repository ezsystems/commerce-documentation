# Orderhistory cookbook

## How to display custom column - e.g. tracking code?

### 1. Check if there is an existing block for your field

By default you can configure columns per document type in the [configuration](orderhistory.md). You can display any field, that a block exists for.

!!! note

    When using the block name as column identifier in the configuration, remove the suffix `_field`.

If there is no block for your custom field, you need to create one.

!!! tip

    Check all the blocks in vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/fields.html.twig.

The block name usually reflects the name of the document line field. Example:

XML definition:

``` xml
<CreditNoteList ses_unbounded="CreditNoteLine">   
    <CreditNoteLine ses_tree="SesExtension">
            ...
            <PaymentMeans>   
                ...         
                <PaymentDueDate>2007-01-01</PaymentDueDate>
            </PaymentMeans>   
    </CreditNoteLine>
</CreditNoteList>
```

Block name:

``` html+twig
{#obj will be passed automatically and it is the document line#}

{% block PaymentMeans_PaymentDueDate_field %}
    {% spaceless %}
        {% set field_value = obj.PaymentMeans.PaymentDueDate.value %}
        {{ block( 'date_field' ) }}
    {% endspaceless %}
{% endblock %}
```

### 2\. Create missing block

If you did not find the corresponding block, you need to create your own. First you need to know the document structure. Custom fields will be passed to the `SesExtension`, if there is no standard for them.

``` xml
<DeliveryNoteList ses_unbounded="CreditNoteLine">   
    <DeliveryNoteListLine ses_tree="SesExtension">
            ...
            <SesExtension>   
                ...         
                <TrackingCode>20057487546987</TrackingCode>
            </SesExtension>   
    </DeliveryNoteListLine>
</DeliveryNoteList>
```

Override the corresponding template:

`vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/fields.html.twig`

Create new block

``` html+twig
{#obj will be passed automatically and it is the document line#}

{% block SesExtension_TrackingCode_field %}
    {% spaceless %}
        {% if obj.SesExtension.value.TrackingCode is defined %}
            {% set field_value = obj.SesExtension.value.TrackingCode %}
            <a href="#">{{ field_value }}</a> 
        {% endif %}        
    {% endspaceless %}
{% endblock %}
```

### 3\. Add new column to the configuration

``` yaml 
siso_order_history:
    #block name is specified here: see more in Components/fields.html.twig
    default_list_fields:    
        delivery_note:
            - ID_list
            - IssueDate
            - SesExtension_TrackingCode
```

## How to override semantic configuration?

### 1\. Make sure bundle is enabled and registered in kernel

Dependency injection will take care of determining the correct value for semantic configuration.

The only requirement is that orderhistory bundle is enabled and registered in Kernel.

### 2\. Custom configuration

Add your own configuration into your yml file.

``` yaml
siso_order_history:
    list:
        max_document_count: 1000
        max_document_count_per_page: 30
```
