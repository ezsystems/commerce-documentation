# Component - Facets

Facets components is a mixture of Foundation extends and our own code. It's put together in file to create a context which is easier to maintain. 

It extends these Foundations components:

- dropdown
- button
- labels

## Sass

**File location**

``` 
scss/storm/_components.facets.scss
```

### Default settings:

``` 
$facet-dropdown-margin-top: rem-calc(-1);

$facet-btn-margin-bottom: rem-calc(5);
$facet-btn-padding: rem-calc(8 30 8 10);
$facet-btn-font-color: $jet;
$facet-btn-bg-color: $smoke;
$facet-btn-border-width: rem-calc(1);
$facet-btn-border-style: solid;
$facet-btn-border-color: $f-dropdown-border-color;
$facet-btn-hover-bg-color: $f-dropdown-border-color;
$facet-btn-hover-font-color: $jet;
$facet-btn-hover-outline: none;
$facet-btn-arrow-position-right: rem-calc(10);
$facet-btn-arrow-bg-color: $oil;
$facet-btn-apply-padding: rem-calc(5 10);
$facet-btn-apply-margin-top: rem-calc(5);
$facet-btn-apply-disabled-bg-color: $aluminum;
$facet-btn-apply-disabled-hover-bg-color: $facet-btn-apply-disabled-bg-color;

$facet-list-padding-right: rem-calc(5);
$facet-list-padding-left: rem-calc(5);
$facet-list-max-height: rem-calc(120);
$facet-list-overflow-y: auto;
$facet-list-mega-max-height: rem-calc(240);
$facet-item-padding-top: rem-calc(4);
$facet-item-padding-bottom: rem-calc(4);
$facet-item-padding-top-large: rem-calc(2);
$facet-item-padding-bottom-large: rem-calc(2);

// item width for different resolutions in case parent has .mega class assigned
$facet-list-mega-item-width-small: 50%;
$facet-list-mega-item-width-large: 33.33%;
$facet-list-mega-item-width-xlarge: 25%;

$facet-search-padding-right: rem-calc(20);
$facet-icon-search-position: absolute;
$facet-icon-search-position-top: rem-calc(12);
$facet-icon-search-position-right: rem-calc(6);

$facet-active-margin: rem-calc(2 2 5);
$facet-active-padding-top: 0;
$facet-active-padding-bottom: rem-calc(3);
$facet-active-line-height: rem-calc(18);
$facet-active-cursor: pointer;

$facet-icon-close-position: relative;
$facet-icon-close-position-top: rem-calc(3);
$facet-icon-close-font-size: rem-calc(20);
$facet-icon-close-font-weight: bold;
$facet-icon-close-rounded-position: absolute;
$facet-icon-close-rounded-position-top: rem-calc(12);
$facet-icon-close-rounded-position-right: rem-calc(5);
$facet-icon-close-rounded-width: rem-calc(20);
$facet-icon-close-rounded-height: rem-calc(20);
$facet-icon-close-rounded-padding: 0;
$facet-icon-close-rounded-margin: 0;
$facet-icon-close-rounded-border-radius: 50%;
$facet-icon-close-rounded-cursor: pointer;

$facet-active-group-position: relative;
$facet-active-group-display: inline-block;
$facet-active-group-padding: rem-calc(0 5 2);
$facet-active-group-margin: rem-calc(5 10 15 0);
$facet-active-group-margin-top-large: 0;
$facet-active-group-border-width: rem-calc(1);
$facet-active-group-border-style: solid;
$facet-active-group-border-color: $flat-clouds;
$facet-active-group-multi-padding-right: rem-calc(33);
$facet-active-group-title-display: table;
$facet-active-group-title-margin: rem-calc(-8 0 0 );
$facet-active-group-title-font-size: rem-calc(12);
$facet-active-group-title-font-weight: normal;
$facet-active-group-title-bg-color: $white;
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Facets section.

## Twig

### Custom code

#### Items list

It's a list of items inside a single facet box.

- c-facets__items-list
- c-facets__item  

``` 
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Search/search_facets.html.twig
 
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
```

By default it's just one column list with radio or checkbox form field. 

In case of long list of items we have introduced a **mega** list which goes full width and have columns. Number of columns depends on the window width.

This is important: 

``` 
{% if facet.facetOptions|length > mega_visibility_limit %}mega{% else %}
```

Whole block including code above.

``` 
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

```

#### Search field

When there is a lot of items in the facet box we show a search box which filters the list.

- c-facets__search-field
- c-facets__search-icon  

``` 
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Search/search_facets.html.twig
 
{% if facet.facetOptions|length > items_visibility_limit %}
  <div class="c-facets__inner-filter u-position-relative">
    <input class="c-facets__search-field js-search-facets-filter-field" type="text"/>
    <i class="fa fa-search c-facets__search-icon"></i>
  
{% endif %}
```

#### Active facets groups

Active facets are grouped with a small box with a group title.

- c-facet__active-group
- c-facet__active-group–multi
- c-facet__active-group-title  

``` 
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Search/search_facets.html.twig

<div class="c-facet__active-group{% if selectedOptions|length > 1 %} c-facet__active-group--multi{% endif %} js-search-facets-active-group">
    <h6 class="c-facet__active-group-title">
        {{ facet.labelKey|st_translate('search') }}
    </h6>
    ...

```

A groups gets c-facet__active-group–multi CSS class when there is more than one selected option.


### Foundation's extends

#### Dropdown

- c-facets__dropdown

``` 
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Search/search_facets.html.twig
 
<div class="f-dropdown {% if facet.facetOptions|length > mega_visibility_limit %}mega{% else %}smaller{% endif %} c-facets__dropdown u-padding-1x js-facets-box"
     id="drop-{{ facet.facetIdentifier }}"
     data-dropdown-content aria-hidden="true">
  ...

```

#### Button

- c-facets__button–apply

``` 
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Search/search_facets.html.twig
 
<button class="button expand disabled c-facets__button--apply js-search-facets-btn-apply"
        disabled>{{ 'apply'|st_translate('search') }}
</button>
```

#### Label

- c-facets__active
- c-facets__icon-close
- c-facets__icon-close–rounded

``` 
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Search/search_facets.html.twig
 
<span class="label c-facets__active js-search-facets-remove"
      data-facet="{{ facetOption.optionIdentifier }}">
  {{ facetOption.optionLabel }}
  <i class="c-facets__icon-close">&times;</i>
...
{% if selectedOptions|length > 1 %}
   <i class="label alert c-facets__icon-close c-facets__icon-close--rounded js-search-facets-active-group-remove" title="{{ 'remove_this_group'|st_translate('search')
}}" data-tooltip data-options="disable_for_touch:true;">&times;</i>
{% endif %}
```
