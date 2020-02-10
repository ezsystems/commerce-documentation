#  Component - Toggler 

We use toggler component to toggle content visibility. It's a well know pattern on many websites and online shops. 

## Sass

**File location**

``` 
scss/storm/_components.toggler.scss
```

### Default settings:

``` 
$toggler-text-align: center;
$toggler-line-color: #999;
$toggler-line-height: 1px;
$toggler-trigger-font-size: rem-calc(14);
$toggler-trigger-padding: rem-calc(5);
$toggler-trigger-bg-color: $white;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Toggler section.

## Available classes

  - c-toggler
  - c-toggler\_\_trigger

## JavaScript part:

[Toggler](Toggler_23560874.html)

## HTML

``` 
<div class="c-toggler">
  <a href="#" class="c-toggler__trigger js-toggler" 
  data-class="hidden"
  data-target="js-toggler-item"
  data-parent="js-toggler-wrapper"
  data-text-swap="Show lesss">Show more</a>
<div class="js-toggler-wrapper">
  <div class="js-toggler-item">Content goes here
  <div class="js-toggler-item">Content goes here
  <div class="js-toggler-item">Content goes here
  <div class="js-toggler-item hidden">Content goes here
  <div class="js-toggler-item hidden">Content goes here
  <div class="js-toggler-item hidden">Content goes here

```

A .c-toggler\_\_trigger needs to paired up with the .js-toggler class in order to trigger some JavaScript magic. 

## Twig

``` 
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Search/search_facets.html.twig
 
<div class="js-facets-group">
  {% for facet in facetGroup.facets %}
    {% if facet.facetOptions|length > 0 %}
      <button href="#" data-dropdown="drop-{{ facet.facetIdentifier }}"
      aria-controls="drop{{ loop.index }}"
      aria-expanded="false"
      class="button small dropdown c-facets__button{% if loop.index > boxes_visibility_limit %} hidden js-search-facets-hidden{% endif %}">{{ facet.labelKey|st_translate('search') }}
    </button>
    <div class="f-dropdown {% if facet.facetOptions|length > mega_visibility_limit %}mega{% else %}smaller{% endif %} c-facets__dropdown u-padding-1x js-facets-box"
     id="drop-{{ facet.facetIdentifier }}"
     data-dropdown-content aria-hidden="true">
     {% if facet.facetOptions|length > items_visibility_limit %}
      <div class="c-facets__inner-filter u-position-relative">
        <input class="c-facets__search-field js-search-facets-filter-field" type="text"/>
        <i class="fa fa-search c-facets__search-icon"></i>
      
    {% endif %}
    <ul class="no-bullet c-facets__items-list">
      {% for facetOption in facet.facetOptions %}
        <li class="c-facets__item c-pair-wrapper js-search-facets-filter-item">
          <input class="u-no-margin{% if facetOption.isSelected %} js-selected{% endif %}"
          id="{{ facetOption.optionIdentifier }}"
          name="{{ facetOption.optionName }}"
          type="{% if facet.isMulti %}checkbox{% else %}radio{% endif %}"
          value="{{ facetOption.optionValue }}"
          {% if facetOption.isSelected %}checked="checked"{% endif %}
            />
            <label class="u-no-margin" for="{{ facetOption.optionIdentifier }}">
              {{ facetOption.optionLabel }}{% if facetOption.numFound > 0 %} ({{ facetOption.numFound }}) {% endif %}
            </label>
          </li>
        {% endfor %}
      </ul>
      {% if facet.isMulti %}
        <button class="button expand disabled c-facets__button--apply js-search-facets-btn-apply"
        disabled>{{ 'apply'|st_translate('search') }}
      </button>
    {% endif %}
  
{% endif %}
{% endfor %}
{% if facetGroup.facets|length > boxes_visibility_limit %}
  <div class="c-toggler">
    <a href="#" class="c-toggler__trigger js-toggler" data-class="hidden"
    data-target="js-search-facets-hidden"
    data-parent="js-facets-group"
    data-text-swap="{{ 'show_less_facets'|st_translate('search') }}">{{ 'show_more_facets'|st_translate('search') }}</a>
  
{% endif %}

```
