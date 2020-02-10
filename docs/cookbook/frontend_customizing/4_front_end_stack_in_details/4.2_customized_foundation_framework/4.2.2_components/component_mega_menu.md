#  Component - Mega menu 

Mega menu component is a combination of custom code and top navigation Foundation component.

## Sass

**File location**

``` 
scss/storm/_components.megamenu.scss
```

### Default settings:

``` 
$megamenu-highlight-bg: lighten($secondary-color, 10%);
$megamenu-highlite-bg: $megamenu-highlight-bg;  // used for legacy purposes
$megamenu-link-bg-color: $megamenu-highlight-bg;

$megamenu-1st-level-height: rem-calc(61);
$megamenu-1st-level-hover-bg-color: $white;
$megamenu-1st-level-hover-font-color: $primary-color;
$megamenu-1st-level-arrow-color: $primary-color;
$megamenu-1st-level-arrow-top: ($topbar-height / 2) + rem-calc(8);
$megamenu-1st-level-item-bg-color: none;
$megamenu-1st-level-item-border-right-width: 1px;
$megamenu-1st-level-item-border-right-style: solid;
$megamenu-1st-level-item-border-right-color: $secondary-color;
$megamenu-1st-level-link-font-size: rem-calc(16);
$megamenu-1st-level-link-font-color: $primary-color;
$megamenu-1st-level-link-padding-top: rem-calc(8);
$megamenu-1st-level-link-padding-bottom: rem-calc(8);
$megamenu-1st-level-link-active-bg-color: $megamenu-highlight-bg;
$megamenu-1st-level-link-active-font-color: $primary-color;
$megamenu-1st-level-link-active-hover-bg-color: $megamenu-highlight-bg;
$megamenu-1st-level-link-active-hover-font-color: $primary-color;
$megamenu-1st-level-link-active-arrow-color: $primary-color;

$megamenu-2nd-level-bg-color: $megamenu-highlight-bg;
$megamenu-2nd-level-parent-width: 100%;
$megamenu-2nd-level-parent-padding-left: rem-calc(10);
$megamenu-2nd-level-stretched-bg-color: $megamenu-highlight-bg; // set to transparent in order to remove
$megamenu-2nd-level-item-margin-right: rem-calc(20);
$megamenu-2nd-level-link-padding: rem-calc(0 10);
$megamenu-2nd-level-link-font-size: rem-calc(20);
$megamenu-2nd-level-link-font-color: $primary-color;
$megamenu-2nd-level-width: 20%;
$megamenu-2nd-level-link-active-bg-color: $flat-peter-river;
$megamenu-2nd-level-link-active-font-color: $white;
$megamenu-2nd-level-link-active-hover-bg-color: $flat-peter-river;
$megamenu-2nd-level-link-active-hover-font-color: $white;
//
$megamenu-3rd-level-padding-left: 0;
$megamenu-3rd-level-border-top-width: 1px;
$megamenu-3rd-level-border-top-style: solid;
$megamenu-3rd-level-border-top-color: $primary-color;
$megamenu-3rd-level-link-padding-left: rem-calc(10);
$megamenu-3rd-level-link-font-size: rem-calc(15);
$megamenu-3rd-level-link-line-height: rem-calc(48);
$megamenu-3rd-level-link-border-bottom: none;
$megamenu-3rd-level-link-hover-font-color: $white;
$megamenu-3rd-level-link-hover-bg-color: scale-color($flat-peter-river, $lightness: 40%);
$megamenu-3rd-level-link-active-bg-color: scale-color($flat-peter-river, $lightness: 40%);
$megamenu-3rd-level-link-active-font-color: $white;
$megamenu-3rd-level-link-active-hover-bg-color: scale-color($flat-peter-river, $lightness: 40%);
$megamenu-3rd-level-link-active-hover-font-color: $white;
$megamenu-4th-level-link-padding-left: rem-calc(30);
$megamenu-4th-level-link-font-size: rem-calc(13);
$megamenu-4th-level-link-hover-font-color: $oil;
$megamenu-4th-level-link-hover-bg-color: scale-color($flat-peter-river, $lightness: 80%);
$megamenu-4th-level-link-active-bg-color: scale-color($flat-peter-river, $lightness: 80%);
$megamenu-4th-level-link-active-font-color: $oil;
$megamenu-4th-level-link-active-hover-bg-color: scale-color($flat-peter-river, $lightness: 80%);
$megamenu-4th-level-link-active-hover-font-color: $oil;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Mega menu section.

### HTML

``` 
<nav class="top-bar c-megamenu--horizontal" data-topbar="" role="navigation" data-options="sticky_on: large">
  <ul class="title-area show-for-large-up">
    <li class="name"></li>
    <li class="toggle-topbar menu-icon"><a href="#">Menu</a></li>
  </ul>
  <section class="top-bar-section show-for-large-up">
    <ul>
      <li class="has-dropdown not-click">
        <a href="/Produkt-Katalog">Produkt Katalog</a>
        <ul class="dropdown">
          <li class="title back js-generated">
            <h5><a href="javascript:void(0)">Back</a></h5></li>
          <li class="parent-link hide-for-medium-up"><a class="parent-link js-generated" href="/Produkt-Katalog">Produkt Katalog</a></li>
          <li class="has-dropdown not-click">
            <a href="/Produkt-Katalog/Lautsprecher">Lautsprecher</a>
            <ul class="dropdown">
              <li class="title back js-generated">
                <h5><a href="javascript:void(0)">Back</a></h5></li>
              <li class="parent-link hide-for-medium-up"><a class="parent-link js-generated" href="/Produkt-Katalog/Lautsprecher">Lautsprecher</a></li>
              <li>
                <a href="/Produkt-Katalog/Lautsprecher/Mc-Serie">Mc Serie</a>
              </li>
              <li>
                <a href="/Produkt-Katalog/Lautsprecher/Xi-Serie">Xi Serie</a>
              </li>
              <li>
                <a href="/Produkt-Katalog/Lautsprecher/Best-of-DAP">Best of DAP</a>
              </li>
            </ul>
          </li>
          <li class="has-dropdown not-click">
            <a href="/Produkt-Katalog/Mikrofone">Mikrofone</a>
            <ul class="dropdown">
              <li class="title back js-generated">
                <h5><a href="javascript:void(0)">Back</a></h5></li>
              <li class="parent-link hide-for-medium-up"><a class="parent-link js-generated" href="/Produkt-Katalog/Mikrofone">Mikrofone</a></li>
              <li>
                <a href="/Produkt-Katalog/Mikrofone/CM-Serie">CM Serie</a>
              </li>
              <li>
                <a href="/Produkt-Katalog/Mikrofone/URM-Serie">URM Serie</a>
              </li>
              <li>
                <a href="/Produkt-Katalog/Mikrofone/Sonderedition">Sonderedition</a>
              </li>
            </ul>
          </li>
          <li class="has-dropdown not-click">
            <a href="/Produkt-Katalog/Beleuchtung">Beleuchtung</a>
            <ul class="dropdown">
              <li class="title back js-generated">
                <h5><a href="javascript:void(0)">Back</a></h5></li>
              <li class="parent-link hide-for-medium-up"><a class="parent-link js-generated" href="/Produkt-Katalog/Beleuchtung">Beleuchtung</a></li>
              <li>
                <a href="/Produkt-Katalog/Beleuchtung/Veranstaltungen">Veranstaltungen</a>
              </li>
              <li>
                <a href="/Produkt-Katalog/Beleuchtung/Highlights">Highlights</a>
              </li>
              <li>
                <a href="/Produkt-Katalog/Beleuchtung/XS-Serie">XS Serie</a>
              </li>
            </ul>
          </li>
          <li class="has-dropdown not-click">
            <a href="/Produkt-Katalog/MiPow">MiPow</a>
            <ul class="dropdown">
              <li class="title back js-generated">
                <h5><a href="javascript:void(0)">Back</a></h5></li>
              <li class="parent-link hide-for-medium-up"><a class="parent-link js-generated" href="/Produkt-Katalog/MiPow">MiPow</a></li>
              <li>
                <a href="/Produkt-Katalog/MiPow/Mobile-Ladegeraete">Mobile Ladegeräte</a>
              </li>
              <li>
                <a href="/Produkt-Katalog/MiPow/Powercube">Powercube</a>
              </li>
            </ul>
          </li>
        </ul>
      </li>
      <li>
        <a href="/Mipow-external-batteries">Mipow external batteries</a>
      </li>
      <li class="has-dropdown not-click">
        <a href="/Service">Service </a>
        <ul class="dropdown">
          <li class="title back js-generated">
            <h5><a href="javascript:void(0)">Back</a></h5></li>
          <li class="parent-link hide-for-medium-up"><a class="parent-link js-generated" href="/Service">Service </a></li>
          <li>
            <a href="/Service/Kontakt">Kontakt</a>
          </li>
          <li>
            <a href="/Service/Ueber-uns">Über uns</a>
          </li>
          <li>
            <a href="/Service/Kaufen">Kaufen</a>
          </li>
        </ul>
      </li>
      <li>
        <a href="/Angebote">Angebote</a>
      </li>
    </ul>
  </section>
</nav>
```

## Twig

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig:216**

``` 
...
<nav class="top-bar c-megamenu--horizontal" data-topbar role="navigation" data-options="sticky_on: large">
  <ul class="title-area show-for-large-up">
    <li class="name"></li>
    <li class="toggle-topbar menu-icon"><a href="#">Menu</a></li>
  </ul>
  <section class="top-bar-section show-for-large-up">
      {{ render_esi(
      controller(
      'SilversolutionsEshopBundle:Navigation:showTopNavigation',
      {'locationId' : 2, 'depth' : 3}
      ),
      {}
      ) }}
  </section>
</nav>
...
```

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/Navigation/top\_navigation.html.twig**

``` 
{% set node = node is defined ? node : ses_navigation({'location_id': 2, 'depth': 3}) %}
{% if node %}
<ul{% if isDropdown %} class="dropdown"{% endif %}>
    {% for child in node.children %}
    {% if child.isNavigationPart %}
        <li{% if child.hasChildren %} class="has-dropdown"{% endif %}>
            <a href="{{ child.url }}">{{ child.name }}</a>
            {% if child.hasChildren %}
                {{ include('SilversolutionsEshopBundle:Catalog/Navigation:top_navigation.html.twig'|st_resolve_template,
                    {'node': child, 'isDropdown': child.hasChildren ? true : false}
                ) }}
            {% endif %}
        </li>
    {% endif %}
    {% endfor %}
</ul>
{% endif %}
```
