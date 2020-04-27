# Breadcrumbs templates

### Templates list

| Path                                                                                      | Description                                                                                                                                             |
| ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig                   | Defines the `breadcrumb` block, which calls the sub-controller for the breadcrumbs generation. This block could be overridden by extending templates. |
| Silversolutions/Bundle/EshopBundle/Resources/views/Breadcrumbs/breadcrumb\_list.html.twig | Template, which is used by all WhiteOctober based breadcrumbs generators, in order to render the generated breadcrumbs elements into HTML.              |

`Breadcrumbs/breadcrumb_list.html.twig`:

``` html+twig
<div class="crumb-wrap hide-for-print">
  <div class="row">
    <div class="columns">
      {% if wo_breadcrumbs()|length %}
        <ul class="breadcrumbs u-no-margin show-for-large-up" itemscope itemtype="http://schema.org/BreadcrumbList">
          {% for crumb in breadcrumbs %}
            {% if not loop.last %}
              <li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
                <a itemprop="item" href="{{ crumb.url }}">
                  <span itemprop="name">{{ crumb.text }}
                </a>
                <meta itemprop="position" content="{{ loop.index }}" />
              </li>
            {% else %}
              {# TODO make last element link configurable #}
              <li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
                <span itemprop="name">{{ crumb.text }}
                <meta itemprop="position" content="{{ loop.index }}" />
              </li>
            {% endif %}
          {% endfor %}
        </ul>
      {% endif %}
```

### Related custom Twig modifiers/functions/etc/:

`wo_breadcrumbs()` function is used in `breadcrumb_list.html.twig` in order to determine the count of generated breadcrumbs. Furthermore the breadcrumbs variable, used in this template, is passed by the BreadcrumbsHelper.

### Additional data in the breadcrumb nodes

"crumb.translationParameters" contains additional data which can be used to change the behavior of the breadcrumb depending on the stored data:

#### CatalogBreadcrumbsGenerator.php

``` php
'TYPE' => STRING 'CATALOG' (LENGTH=7)
'IDENTIFIER' => STRING '9686' (LENGTH=4)
'CONTENT_TYPE_ID' => STRING '' (LENGTH=0)
```

#### EzContentBreadcrumbsGenerator.php

``` php
'TYPE' => STRING 'EZ_CONTENT' (LENGTH=10)
'IDENTIFIER' => INT 57
'CONTENT_TYPE_ID' => INT 23
```

#### RoutesBreadcrumbsGenerator.php

``` php
'TYPE' => STRING 'ROUTE' (LENGTH=5)
'IDENTIFIER' => STRING '' (LENGTH=0)
'CONTENT_TYPE_ID' => STRING '' (LENGTH=0)
```

Example for not clickable catalog breadcrumb in template:

`breadcrumb_list.html.twig`:

``` html+twig
{% if crumb.translationParameters.type == 'catalog' %}
  <span itemprop="name">{{ crumb.text }}
  <meta itemprop="position" content="{{ loop.index }}" />
{% else %}
  <a itemprop="item" href="{{ crumb.url }}">
    <span itemprop="name">{{ crumb.text }}
  </a>
  <meta itemprop="position" content="{{ loop.index }}" />
{% endif %}
```
