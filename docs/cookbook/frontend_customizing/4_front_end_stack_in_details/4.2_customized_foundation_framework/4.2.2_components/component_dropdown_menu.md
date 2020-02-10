#  Component - Dropdown menu 

Dropdown menu component is a combination of custom code and top navigation Foundation component.

## Sass

**File location**

``` 
scss/storm/_components.dropdownmenu.scss
```

### Default settings:

``` 
$dropdown-primary-color: $primary-color;
$dropdown-secondary-color: #333 ;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Dropdown menu section.

### HTML

``` 
<nav class="top-bar c-dropdownmenu" data-topbar="" role="navigation" data-options="sticky_on: large">
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
<nav class="top-bar c-dropdownmenu" data-topbar role="navigation" data-options="sticky_on: large">
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
