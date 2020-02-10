#  Orderhistory - API 

### Order search form

The order search form is using the DatePicker plugin for better user experience.  
If you need to pass some additional parameters for this plugin, or adapt the form attributes, you need to override the form type and form validation service.

![](attachments/23560628/23563464.png)

#### Form type - OrderType

    vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Form/Type/OrderType.php

**Service definition**

``` 
<parameter key="siso_order_history.order_type.class">Siso\Bundle\OrderHistoryBundle\Form\Type\OrderType</parameter>

<service id="siso_order_history.order_type" class="%siso_order_history.order_type.class%" scope="prototype">
    <argument type="service" id="ezpublish.config.resolver" />
    <argument>%siso_order_history.date%</argument>
</service> 
```

``` 
//Example: pass some data-attributes for the datepicker plugin

$builder->add('orders_from', 'text', array(
        'label' => 'Orders from:',
        'required' => true,
        'data' => $options['data']['from_date'],
        'attr' => array(
            'class' => 'datepicker',
            'data-date-direction-start' => $maxDirectionStart,
            'data-date-direction-end' => $maxDirectionEnd,
            'data-date-caption-days' => 'F Y',
            'data-date-format' => $dateFormat
        )
    )
);
```

#### Form validation - FormValidationService

    vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Services/FormValidationService.php

**Service definition**

``` 
<parameter key="siso_order_history.form_validation.class">Siso\Bundle\OrderHistoryBundle\Services\FormValidationService</parameter>

<service id="siso_order_history.form_validation" class="%siso_order_history.form_validation.class%">
    <argument type="service" id="siso_order_history.date_time"/>
    <argument type="service" id="ezpublish.config.resolver"/>
</service>
```

``` 
// adjust validation
public function isValid($params) { }
```

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-10-12 um 15.36.07.png](attachments/23560628/23563464.png) (image/png)  
