# Component - Variants

We use this component to display variants on product detail and basket (edit) page.

Important JS classes

- js-variants-wrapper

## Sass

Needs to be refactored using our coding standards.

**File location**

``` 
scss/storm/_components.variants.scss
```

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/productVariantBlock.html.twig**

``` 
<div class="js-variants-wrapper" data-variant-sku="{{ catalogElement.sku }}">
    {% set sortedCharacterictics = catalogElement.variantCharacteristics.characteristics|sort_characteristics(catalogElement.type) %}
    {% for characteristics in sortedCharacterictics %}
        {% for variantIndex, variantCharacteristic in characteristics %}
            {% set variantCharacteristic = variantCharacteristic|sort_characteristic_codes(variantIndex) %}
            <div class="variant-group js-variant-group">
                <strong>{{ variantCharacteristic.label }}</strong>
                <ul class="u-no-margin-left u-marign-bottom-1x variant-list js-variant-list" data-variant-group="{{ variantIndex }}">
                    {% for code, codeValue in variantCharacteristic.codes %}

                        {# check if variant is available #}
                        {% if availableVariantCodes is not defined or availableVariantCodes[variantIndex][code] is defined %}
                            {% set availableClass = ' available' %}
                        {% else %}
                            {% set availableClass = ' unavailable' %}
                        {% endif %}

                        {# add active class depending on data #}
                        {% set activeClass = '' %}
                        {% if data.variants[variantIndex] is defined and data.variants[variantIndex] == code %}
                            {% set activeClass = ' active' %}
                        {% endif %}

                        <li class="{{ code }}{{ availableClass }}{{ activeClass }}">
                            <a href="#" title="{{variantCharacteristic.label }}: {{ codeValue }}" data-variant-code="{{ code }}" >
                                {% set useValue = true %}
                                {% for imageCode, imageRecord in variantCharacteristic.images %}
                                    {% if imageCode == code %}
                                        <img src="/{{ st_imageconverter(imageRecord, 'original') }}" />
                                        {% set useValue = false %}
                                    {% endif %}
                                {% endfor %}
                                {% if useValue %}
                                    {{ codeValue }}
                                {% endif %}
                            </a>
                        </li>
                    {% endfor %}
                </ul>
            
        {% endfor %}
    {% endfor %}

```
