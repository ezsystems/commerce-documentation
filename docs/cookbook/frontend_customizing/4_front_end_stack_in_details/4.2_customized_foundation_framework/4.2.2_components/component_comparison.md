# Component - Comparison

Comparison component is based on pixel height to adjust the elements to be on the same level. The comparison is only visible on desktop to get the best user experience.

Needs to be refactored using our code standards

## Third Party Plugin

We are using third party plugin called Sortable (http://rubaxa.github.io/Sortable/) to have drag & drop functionality

``` 
vendor/Sortable
```

## JavaScript

**File location**

``` 
js/pages/comparison/storm.comparison.js
```

## Sass

**File location**

``` 
scss/storm/_components.comparison.scss
```

### Default settings:

``` 
$comparison-list-background: $primary-color;
$comparison-list-color: $white;
$comparsion-list-padding: rem-calc(10);

$comparison-paginator-arrow-color: $primary-color;

$comparison-data-headline-color: $primary-color;
$comparison-data-headline-arrow-color: $primary-color;

$comparison-item-background: $white;

$comparison-data-table-li-padding: rem-calc(5 15);
$comparison-data-table-li-background: $white;
$comparison-data-table-li-color: $primary-color;
$comparison-data-table-li-odd-background: $flat-clouds;
$comparison-data-table-li-odd-color: $primary-color;
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Comparison section.

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Comparison/comparison_list.html.twig**

``` 
 <div class="show-for-large-up">
  <div class="row">
    {% if comparisonList is not empty %}

      {% for basket in comparisonList %}
        {% set error = basket.errorMessages %}
        {% block messages %}
          {{ include('SilversolutionsEshopBundle:Basket:messages.html.twig'|st_resolve_template) }}
        {% endblock %}
      {% endfor %}

      <div class="medium-3 columns box-left"{% if sort_enabled %} data-sort-enable="true"{% endif %}>
        <div class="c-comparison__list">
          {% if ses_config_parameter('collapse_attributes','siso_comparison') %}
            <h4>
              <a href="#" class="js-attributes-trigger" data-text-swap="{{ 'Only differences'|st_translate }}">
                <i class="fa fa-toggle-on"></i> {{ 'All Attributes'|st_translate }}
              </a>
            </h4>
          {% endif %}

          <h4>{{ 'Your List'|st_translate }}</h4>

          <ul class="no-bullet js-comparison-tabs">
            {% for basket in comparisonList %}
              <li><a href="#{{ basket.basketId }}" id="tabs-{{ basket.basketId }}"
                     data-tab="{{ basket.basketId }}">{{ basket.basketName|replace({"-" : " "})|st_translate|capitalize }}</a></li>
            {% endfor %}
          </ul>

          <h4>{{ 'This List'|st_translate }}</h4>

          <div class="js-action-list">
            {% for basket in comparisonList %}
              <ul class="no-bullet js-actions" id="actions-{{ basket.basketId }}">
                {# TODO: find a way to display this content adjusted for print #}
                {#<li>#}
                  {#<a href="#{{ basket.basketId }}" class="js-action-print">#}
                    {#<i class="fa fa-print"></i>  {{ 'Print'|st_translate }}#}
                  {#</a>#}
                {#</li>#}
                {% if sort_enabled == true %}
                  <li>
                    <a href="#{{ basket.basketId }}" class="js-remove-stored-basket" data-basket-id="{{ basket.basketId }}">
                      <i class="fa fa-trash"></i> {{ 'Delete'|st_translate }}
                    </a>
                  </li>
                {% endif %}
              </ul>
            {% endfor %}

        <div class="c-comparison__data-attributes">
          {# comparison attributes #}
          <div class="attributes-list">
            {% for basket in comparisonList %}
              {% if basket.comparisonAttributes is not empty %}
                {% set listID = loop.index %}
                <div class="attributes" id="attributes-{{ basket.basketId }}">
                  {% for attributeGroup, attributeNames in basket.comparisonAttributes %}
                    <div{% if attributeNames.collapsed is defined %} class="collapsed"{% endif %}
                      data-accordion-id="{{ listID }}-{{ loop.index }}">
                      <h4{% if ses_config_parameter('collapse_groups','siso_comparison') %} class="js-compare-accordion-trigger"{% endif %}>{{ attributeGroup|st_translate }}</h4>
                      <ul class="{% if attributeNames.collapsed is defined %}hide{% endif %} element-list">
                        {% for attributeName in attributeNames.list %}
                          <li{% if attributeName.collapsed is defined %} class="attribute-collapsed"{% endif %}>
                            {% if attributeName.name is defined %}
                              {{ attributeName.name }}
                            {% else %}
                              -
                            {% endif %}
                          </li>
                        {% endfor %}
                      </ul>
                    
                  {% endfor %}
                
              {% endif %}
            {% endfor %}

      <div class="medium-9 columns">
        <h1>{{ 'My Comparison List'|st_translate }}</h1>
        {% for basket in comparisonList %}
          <div class="box-elements sortable-list-group">
            <div class="box-elements-header">
              {% if basket.comparisonElements|length > 3 %}
                <div class="pagination-wrapper">
                  <a href="#" class="prev" id="prev-{{ loop.index }}">
                    <i class="fa fa-chevron-left"></i>
                  </a>

                  <span class="js-slide-counter">
                    {% spaceless %}
                      <span class="js-slide-count-from">1-<span
                      class="js-slide-count-to">4
                      <span class="js-slide-count-total">/{{ basket.comparisonElements|length }}
                    {% endspaceless %}

                  <a href="#" class="next" id="next-{{ loop.index }}">
                    <i class="fa fa-chevron-right"></i>
                  </a>
                
              {% endif %}

            {% if basket.comparisonElements is not empty %}
              {% set listID = loop.index %}
              <div class="elements sortable-group {% if loop.index > 1 %} hide{% endif %}"
                   id="basket-id-{{ basket.basketId }}" data-basket-id="{{ basket.basketId }}">

                {% for element in basket.comparisonElements %}
                  <div class="left sortable-list-group-item" id="basket-line-{{ element.basketLineId }}"
                       data-basket-line-id="{{ element.basketLineId }}">
                    <div class="sortable-list-group-item-inner">
                      <div class="drag-item"><i class="fa fa-3x fa-arrows sortable-handle"></i>
                      {% if element.catalogElement is not empty %}
                        {% set catalogElement = element.catalogElement %}
                        <div class="element-info">
                          {# show image, name, price, actions, add to cart, availability #}
                          {% if sort_enabled == true %}
                            <div class="comparison-actions">
                              <a href="" class="remove-basketline js-remove-basketline-stored-basket right"
                                 data-basket-id="{{ basket.basketId }}"
                                 data-basket-line-id="{{ element.basketLineId }}">
                                <i class="fa fa-lg fa-trash"></i>
                              </a>
                            
                          {% endif %}
                          <div class="comparison-image">
                            <figure>
                                {% set productId = catalogElement.sku %}
                                {% if catalogElement.variantCode|default is not empty %}
                                    {% set productId = catalogElement.sku ~ catalogElement.variantCode %}
                                {% endif %}
                                {% set mainImage = ses_assets_main_image(catalogElement, productId) %}
                                {# if no image was found for the choosen variant, use main image from the Variant product node #}
                                {% if mainImage is empty and catalogElement.variantCode|default is not empty %}
                                    {% set variantProduct = ses_variant_product_by_sku(catalogElement.sku) %}
                                    {% if variantProduct|default is not empty %}
                                        {% set mainImage = ses_assets_main_image(variantProduct) %}
                                    {% endif %}
                                {% endif %}
                              <a href="{{ catalogElement.seoUrl }}">
                                <img src="/{{ st_imageconverter(mainImage, 'thumb_smaller') }}" alt=""/>
                                <figcaption>{{ catalogElement.name }}</figcaption>
                              </a>
                            </figure>
                            <div class="u-text-smaller text-left">{{ 'SKU'|st_translate }}: {{ catalogElement.sku }}
                            {% if catalogElement.variantCode|default is not empty %}
                              <div class="u-text-smaller text-left">{{ 'Variant'|st_translate }}: {{ catalogElement.variantCode }}
                            {% endif %}

                          <div class="comparison-price text-right">
                            {{ render(
                            controller(
                            'SilversolutionsEshopBundle:Catalog:showCatalogElementPrice',
                            {
                            'catalogElementId': catalogElement.identifier,
                            'priceEngineContextId': 'comparison',
                            'variantCode': catalogElement.variantCode
                            }
                            ),
                            {
                            'strategy': renderParams.priceBlockRenderStrategy
                            }
                            ) }}

                          <form action="{{ path('silversolutions_add_to_basket') }}" method="post">
                              {% if catalogElement.variantCode|default is not empty %}
                              <input type="hidden" name="ses_basket[{{ loop.index }}][isVariant]" value="isVariant"/>
                              <input type="hidden" name="ses_basket[{{ loop.index }}][ses_variant_code]"
                                     value="{{ catalogElement.variantCode }}"/>
                              {% endif %}
                            <input type="hidden" name="ses_basket[{{ loop.index }}][quantity]" class="amount" value="1"
                                   data-product-id="{{ catalogElement.sku }}"
                              {% if catalogElement.variantCharacteristics is defined %} data-is-variant="isVariantcode"{% endif %}>
                            <input type="hidden" name="ses_basket[{{ loop.index }}][sku]" value="{{ catalogElement.sku }}"/>
                            <button name="add_to_basket" type="submit" value="{{ 'Add to basket'|st_translate }}"
                                    class="button expand js-add-to-basket">
                              <i class="fa fa-cart-plus fa-lg"></i>
                              <span class="show-for-xlarge-up">{{ 'Add to basket'|st_translate }}
                            </button>
                          </form>
                        
                      {% endif %}

                      {% if element.attributes is not empty %}
                        <div class="element-attributes">
                          {% for attributeGroup, attributeValues in element.attributes %}
                            <div {% if attributeValues.collapsed is defined %} class="collapsed"{% endif %}
                              data-accordion-id="{{ listID }}-{{ loop.index }}">
                              <h4>&nbsp;</h4>
                              <ul class="{% if attributeValues.collapsed is defined %}hide{% endif %} element-list">
                                {% for attributeName in attributeValues.list %}
                                  <li{% if attributeName.collapsed is defined %} class="attribute-collapsed"{% endif %}>
                                    {% if attributeName.name is not empty %}
                                      {{ attributeName.name }}
                                    {% else %}
                                      -
                                    {% endif %}
                                  </li>
                                {% endfor %}
                              </ul>
                            
                          {% endfor %}
                        
                      {% endif %}

                {% endfor %}
              
            {% endif %}
          
        {% endfor %}
      
    {% endif %}
  
  <div class="row">
    <div class="columns">
      <div class="comparison-message{% if comparisonList is not empty %} hide{% endif %}">
        <div class="alert-box alert" data-alert>
          {{ 'Your Comparison List is empty'|st_translate }}!

<div class="hide-for-large-up">
  <div data-alert class="alert-box info">
    {{ 'Our apologies but comparison feature is optimized for bigger screen size.'|st_translate }}

```
