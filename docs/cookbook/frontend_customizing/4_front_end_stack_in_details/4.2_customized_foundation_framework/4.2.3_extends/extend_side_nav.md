# Extend - Side Nav

Extends: http://foundation.zurb.com/sites/docs/v/5.5.3/components/sidenav.html

## What/why do we extend?

1. We had to extend it in order to make loot better than just a standard list.

## Sass:

**File location**

``` 
scss/storm/_components.extend.side-nav.scss
```

### Default settings:

``` 
$side-nav-border-width: rem-calc(1);
$side-nav-border-color: $iron;
$side-nav-border-style: solid;

$side-nav-element-font-color: $primary-color;
$side-nav-element-background: $white;

$side-nav-margin-bottom: rem-calc(16);

$side-nav-active-element-background: $flat-peter-river;
$side-nav-active-element-color: $white;
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Side Nav section.

### Available classes

``` 
.c-side-nav__next-level
```

## Twig:

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/Navigation/left_navigation.html.twig**

``` 
{% set node = node is defined ? node : ses_navigation({'location_id': 86}) %}
{% set level = level is defined ? level : 1 %}

{% if level == 1 %}
<nav>
  <ul class="side-nav" role="navigation">
{% endif %}

    {% if node %}

      {% if level == 1 %}
        <li class="heading">
          {{ node.name }}
        </li>
      {% endif %}

      {% if level >= 2 %}
        <ul class="no-bullet c-side-nav__next-level">
      {% endif %}

      {% for child in node.children %}
        {% if child.isNavigationPart %}
          <li{% if child.isBreadCrumbPart %} class="active"{% endif %} role="menuitem">
            <a href="{{ child.url }}">{{ child.name }}</a>
            {% if child.hasChildren %}
              {{ include('SilversolutionsEshopBundle:Catalog/Navigation:left_navigation.html.twig'|st_resolve_template,
              {'node': child, 'level': level + 1}
              ) }}
            {% endif %}
          </li>
        {% endif %}
      {% endfor %}

      {% if level >= 2 %}
        </ul>
      {% endif %}

    {% endif %}

{% if level == 1 %}
  </ul>
</nav>
{% endif %}
```

To create a side nav wrap an `<ul>` with a `<nav>`. Main `<ul>` gets `.side-nav` class. Every heading should get a `.heading` class. For second level and deeper you apply `.no-bullet.c-side-nav__next-level`.
