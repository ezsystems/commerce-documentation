# Extend - Tabs

Extends: http://foundation.zurb.com/sites/docs/v/5.5.3/components/tabs.html#tabs-deeplink-3

## What/why do we extend? 

1. Change standard look and feel
1. On small screen size title gets full width
1. Icon that shows opened / closed tab (using Font Awesome)

## Sass

**File location**

``` 
scss/storm/_components.extend.tabs.scss
```

### Default settings:

``` 
$tabs-top-active-color: $primary-color;
$tabs-top-border-width: rem-calc(3);

$tabs-border-width: rem-calc(1);
$tabs-border-style: solid;
$tabs-border-color: $flat-clouds;
$tabs-border-bottom-color: $white;
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Tabs section.

## Twig

### Tabs on the product detail page

``` 
{% block product_tabs %}

  <ul class="tabs" data-tab role="tablist">
    {% set manufacturer = ses_render_field(catalogElement, 'manufacturer') %}
    {% set manufacturerSku = ses_render_field(catalogElement, 'manufacturerSku') %}
    {% set color = ses_render_field(catalogElement, 'color') %}
    {% set specificationMatrix = ses_render_specification_matrix(catalogElement) %}

    {% if ses_render_field(catalogElement, 'longDescription') %}
      <li class="tab-title active" role="presentation">
        <a href="#tab-description" role="tab" aria-selected="true" aria-controls="#tab-description">
          {{ 'Description'|st_translate }}
        </a>
      </li>
    {% endif %}

    {% if manufacturer or manufacturerSku or color or specificationMatrix %}
      <li class="tab-title" role="presentation">
        <a href="#tab-technical" role="tab" aria-selected="false" aria-controls="#tab-technical">
          {{ 'Technical information'|st_translate }}
        </a>
      </li>
    {% endif %}

    {% if ses_render_field(catalogElement, 'video') %}
      <li class="tab-title" role="presentation">
        <a href="#tab-videos" role="tab" aria-selected="false" aria-controls="#tab-videos">
          {{ 'Videos'|st_translate }}
        </a>
      </li>
    {% endif %}

    {% set manuals = ses_assets_by_group(catalogElement, 'Manuals') %}
    {% if manuals != null %}
      <li class="tab-title" role="presentation">
        <a href="#tab-manuals" role="tab" aria-selected="false" aria-controls="#tab-manuals">
          {{ 'Manuals'|st_translate }}
        </a>
      </li>
    {% endif %}
  </ul>

  <div class="tabs-content">

    {% if ses_render_field(catalogElement, 'longDescription') %}
      <section id="tab-description" class="content active" role="tabpanel" aria-hidden="false">
        {{ ses_render_field(catalogElement, 'longDescription') }}
      </section>
    {% endif %}

    {% if manufacturer or manufacturerSku or color or specificationMatrix %}
      <section id="tab-technical" class="content u-no-padding" role="tabpanel" aria-hidden="true">
        <ul class="accordion u-no-margin" data-accordion data-options="multi_expand: true;">
          <li class="accordion-navigation active">
            <a href="#panel-technical">{{ 'Technical information'|st_translate }}</a>

            <div id="panel-technical" class="content active">
              <table width="100%">
                <tbody>
                <tr>
                  <td>{{ 'Manufacturer'|st_translate }}:</td>
                  <td>{{ manufacturer }}</td>
                </tr>
                <tr>
                  <td>{{ 'Manufacturer SKU'|st_translate }}:</td>
                  <td>{{ manufacturerSku }}</td>
                </tr>
                <tr>
                  <td>{{ 'Color'|st_translate }}:</td>
                  <td>{{ color }}</td>
                </tr>
                </tbody>
              </table>
            
          </li>

          {{ specificationMatrix|raw }}

          {% if ses_render_field(catalogElement, 'specification') %}
            <p>
              {{ ses_render_field(catalogElement, 'specification') }}
            </p>
          {% endif %}

        </ul>
      </section>
    {% endif %}

    {% if ses_render_field(catalogElement, 'video') %}
      <section id="tab-videos" class="content" role="tabpanel" aria-hidden="true">
        <h4>{{ 'Videos'|st_translate }}</h4>

        <div class="flex-video">
          <iframe src="{{ ses_render_field(catalogElement, 'video') }}" frameborder="0" allowfullscreen></iframe>
        
        <a href="{{ ses_render_field(catalogElement, 'video') }}">{{ 'View thin on YouTube'|st_translate }}</a>
      </section>
    {% endif %}

    {# get some other assets, like pdfs #}
    {% if manuals != null %}
      <section id="tab-manuals" class="content" role="tabpanel" aria-hidden="true">
        <h4>{{ 'Manuals'|st_translate }}</h4>

        <div class="space_bottom">
          {% for manual in manuals %}
            <a href="{{ asset(manual.path) }}">{{ manual.fileName }}</a>
          {% endfor %}
        
      </section>
    {% endif %}

{% endblock %}
```
