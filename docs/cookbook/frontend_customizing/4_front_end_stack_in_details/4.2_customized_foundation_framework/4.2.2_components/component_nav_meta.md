#  Component - Nav Meta 

Component used for the meta navigation. It extends Foundation inline-list component.

## Sass

**File location**

``` 
scss/storm/_components.nav-meta.scss
```

### Default settings:

``` 
$nav-meta-margin: rem-calc($rem-base/2) 0;
$nav-meta-medium-float: right;
$nav-meta-medium-overflow: visible;
$nav-meta-tiny-dropdown-width: rem-calc(200);
$nav-meta-small-dropdown-width: rem-calc(300);
$nav-meta-medium-dropdown-width: rem-calc(400);
$nav-meta-large-dropdown-width: rem-calc(600);
$nav-meta-huge-dropdown-width: rem-calc(768);

$nav-meta-customer-number-width: 100%;
$nav-meta-customer-number-margin-bottom: rem-calc(16);
$nav-meta-customer-number-line-height: normal;
$nav-meta-customer-number-text-align: left;

$nav-meta-dropdown-trigger-padding-right: rem-calc(15);
$nav-meta-dropdown-trigger-letter-spacing: rem-calc(1);
$nav-meta-dropdown-trigger-text-transform: uppercase;
$nav-meta-dropdown-trigger-arrow-top: rem-calc(8);
$nav-meta-dropdown-trigger-arrow-width: rem-calc(5);
$nav-meta-dropdown-trigger-arrow-border-color: $primary-color;
$nav-meta-dropdown-content-link-font-color: $flat-peter-river;

$nav-meta-dropdown-column-border-width: 1px;
$nav-meta-dropdown-column-border-color: lighten($f-dropdown-border-color, 15%);
$nav-meta-dropdown-column-position-top: ($f-dropdown-content-padding * 4);
$nav-meta-dropdown-column-position-bottom: $f-dropdown-content-padding;
$nav-meta-dropdown-column-margin-left: -($f-dropdown-content-padding);
$nav-meta-dropdown-header-link-margin-left: -($f-dropdown-content-padding);
$nav-meta-dropdown-header-link-padding-left: $f-dropdown-content-padding;
$nav-meta-dropdown-header-link-font-weight: bold;
$nav-meta-dropdown-header-link-text-transform: uppercase;
$nav-meta-dropdown-header-link-border-left-width: 1px;
$nav-meta-dropdown-header-link-border-left-style: solid;
$nav-meta-dropdown-header-link-border-left-color: $f-dropdown-border-color;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Nav Meta section.

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig**

``` 
<div class="meta-nav">
  <div class="row show-for-large-up">
    <div class="columns">
      <ul class="inline-list c-nav-meta">
        <li>

            {% block header_login %}
              {{ render_esi(
              controller(
              'SilversolutionsEshopBundle:CustomerProfileData:showHeaderLogin',
              {'isOffCanvas': false}
              )
              ) }}
            {% endblock %}

        </li>
        <li>
          <a href="#" class="c-nav-meta__dropdown with-arrow" data-dropdown="dropdown-service"  data-options="is_hover:true">
            <i class="fa fa-info-circle"></i>
            {{ 'Service'|st_translate }}
          </a>

              {{ render_esi(
                 controller(
                 'SilversolutionsEshopBundle:Navigation:showServiceMenu',
                 {}
               )
              ) }}

        </li>
          {% block language_switcher %}
            <li>
              <a href="#" class="c-nav-meta__dropdown with-arrow" data-dropdown="dropdown-languages" data-options="is_hover:true">
                <i class="fa fa-language"></i>
                {% if current_siteaccess != 'engl' %}Deutsch{% else %}English{% endif %}
              </a>

              <ul id="dropdown-languages" class="f-dropdown tiny text-left content c-nav-meta__content"
                  data-dropdown-content>
                <li class="current">
                  <a href="{{ url('silversolutions_language_switcher', {'siteaccess' : 'ger', 'source' : source, 'id' : id }) }}">
                    Deutsch
                  </a>
                </li>
                <li>
                  <a href="{{ url('silversolutions_language_switcher', {'siteaccess' : 'engl', 'source' : source, 'id' : id }) }}">
                    English
                  </a>
                </li>
              </ul>
            </li>
          {% endblock %}
      </ul>

```
