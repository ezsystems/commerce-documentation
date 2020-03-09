# Component - Checkout

Checkout page specific component. Includes styling for every step, as well as sidebar.

Important JS classes

- js-checkout-box (used for every checkout step)
- js-checkout-invoice-box (example - used for "scroll to" function)

## Sass

**File location**

``` 
scss/storm/_components.checkout.scss
```

We use 3 different states (CSS classes) for going through the checkout progress. Step templates that are listed below.

1. not-done (grey)
1. progress (orange)
1. done (green)

## Twig template structure

- checkout.html.twig (container)
  - checkout_login.html.twig (step)
  - checkout_invoice_address.html.twig (step)
  - checkout_delivery_address.html.twig (step)
  - checkout_shipping_payment.html.twig (step)
  - checkout_summary.html.twig (step)
  - sidebar_invoice_address.html.twig (sidebar)
  - sidebar_delivery_address.html.twig (sidebar)
  - sidebar_summary.html.twig (sidebar)

## Twig step example (invoice address)

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_invoice_address.html.twig**

``` 
<div class="js-checkout-box js-checkout-invoice-box{% if params.visible is empty %} collapsed{% endif %}" id="box_invoice">
  <div class="checkout-step {{ params.css }}">

    <h4>
      {% if params.editable is not empty %}<i class="fa fa-check"></i>{% endif %} {{ 'Invoice Address'|st_translate }}
      <small>
        {% if params.editable is empty %}
          {% if ses.profile.sesUser.customerNumber|default is empty %}
            {#<span class="u-block-on-medium u-float-right-on-large">{{ 'Please enter your address'|st_translate }}#}
          {% endif %}
        {% else %}
          <a class="button secondary tiny u-float-right-on-medium u-no-margin js-edit-step" href="#invoice">
            {% if ses.profile.sesUser.customerNumber %}
              {{ 'Show'|st_translate }}
            {% else %}
              {{ 'Edit'|st_translate }}
            {% endif %}
          </a>
        {% endif %}
      </small>
    </h4>

  {% if params.visible %}

    {% if form is defined %}

      {% if msg|default is not empty %}
        <div data-alert class="alert-box alert u-margin-top-1x">
          {{ msg|st_translate }}
        
      {% endif %}

      <form action="{{ path('siso_checkout_homepage') }}" method="post" {{ form_enctype(form) }} id="form_invoice">
        <fieldset>
          {{ form_errors(form) }}
          <div {% if form.company.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.company) }}
            {{ form_widget(form.company, { 'attr': { 'minlength': 2, 'maxlength': '30' } }) }}
            {% for error in form.company.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}
          
          <div {% if form.companySecond.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.companySecond) }}
            {{ form_widget(form.companySecond, { 'attr': { 'minlength': 2, 'maxlength': '30' } }) }}
            {% for error in form.companySecond.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}

          <div {% if form.street.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.street) }}
            {{ form_widget(form.street, { 'attr': { 'minlength': 2, 'maxlength': '30' } }) }}
            {% for error in form.street.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}

          <div {% if form.addressSecond.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.addressSecond) }}
            {{ form_widget(form.addressSecond, { 'attr': { 'minlength': 2, 'maxlength': '30' } }) }}
            {% for error in form.addressSecond.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}

          <div {% if form.zip.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.zip) }}
            {{ form_widget(form.zip, { 'attr': { 'minlength': 3, 'maxlength': '20' } }) }}
            {% for error in form.zip.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}

          <div {% if form.city.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.city) }}
            {{ form_widget(form.city, { 'attr': { 'minlength': 2, 'maxlength': '30' } }) }}
            {% for error in form.city.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}

          <div {% if form.country.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.country) }}
            {{ form_widget(form.country) }}
            {% for error in form.country.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}

          <div {% if form.county.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.county) }}
            {{ form_widget(form.county, { 'attr': { 'minlength': 2, 'maxlength': '30' } }) }}
            {% for error in form.county.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}

          <div {% if form.phone.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.phone) }}
            {{ form_widget(form.phone) }}
            {% for error in form.phone.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}

          <div {% if form.email.vars.errors is not empty %} class="error"{% endif %}>
            {{ form_label(form.email) }}
            {{ form_widget(form.email) }}
            {% for error in form.email.vars.errors %}
              <small class="error">{{ error.message }}</small>
            {% endfor %}

          {% if ses.profile.sesUser.customerNumber is empty %}
            <div class="{% if form.invoiceSameAsDelivery.vars.errors is not empty %} error{% endif %}">
              <div class="c-pair-wrapper">
                {{ form_widget(form.invoiceSameAsDelivery) }}
                {{ form_label(form.invoiceSameAsDelivery) }}
              
              {% for error in form.invoiceSameAsDelivery.vars.errors %}
                <small class="error">{{ error.message }}</small>
              {% endfor %}
            
          {% endif %}

          {% do form.invoiceSameAsDelivery.setRendered %}
          {{ form_rest(form) }}

          * {{ 'required fields'|st_translate }}
          {% if form.invoiceSameAsDelivery.vars.checked %}
            <button type="submit" name="send" class="button right"
                    data-toggle-text="{{ 'Continue to delivery'|st_translate }}">{{ 'Continue to shipping and payment'|st_translate }}</button>
          {% else %}
            <button type="submit" name="send" class="button right"
                    data-toggle-text="{{ 'Continue to shipping and payment'|st_translate }}">{{ 'Continue to delivery'|st_translate }}</button>
          {% endif %}
        </fieldset>
      </form>
    {% endif %}
  {% endif %}

```

## Twig settings for states

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_invoice_address.html.twig**

``` 
{% if current_step == 'invoice' %}
  {% set params = box_active|merge({'css' : 'progress'}) %}
{% elseif current_step in ['delivery','shippingPayment','summary'] %}
  {% set params = box_prev|merge({'css' : 'done'}) %}
{% else %}
  {% set params = box_next|merge({'css' : 'not-done'}) %}
{% endif %}
```
