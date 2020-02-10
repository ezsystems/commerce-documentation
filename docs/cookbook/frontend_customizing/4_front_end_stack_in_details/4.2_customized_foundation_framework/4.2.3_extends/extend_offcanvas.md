#  Extend - Offcanvas 

Extends: <http://foundation.zurb.com/sites/docs/v/5.5.3/components/offcanvas.html>

## What/why do we extend?

1.  We need more components than standard offcanvas can offer:
    1.  login form / user area
    2.  language switcher
    3.  meta navigation content
    4.  catalog
2.  Height set to 100% in order to cover full height column.
3.  Hamburger menu icon extended.  

## Sass:

**File location**

``` 
scss/storm/_components.extend.offcanvas.scss
```

### Default settings:

``` 
$off-canvas-first-level-bg: lighten($jet, 15%);

$off-canvas-hamburger-position: relative;
$off-canvas-hamburger-zindex: 1;
$off-canvas-hamburger-line-height: rem-calc(40);
$off-canvas-hamburger-color: $primary-color;
$off-canvas-hamburger-hover-color: $secondary-color;

$off-canvas-dropdown-border-color: scale-color($off-canvas-bg, $lightness: 12%);
$off-canvas-dropdown-link-padding: rem-calc(0 5);
$off-canvas-dropdown-link-color: $oil;
$off-canvas-dropdown-link-bg-color: $white;

$off-canvas-accordion-nav-padding: $off-canvas-link-padding;
$off-canvas-accordion-nav-color: $off-canvas-link-color;
$off-canvas-accordion-nav-bg-color: $off-canvas-first-level-bg;
$off-canvas-accordion-nav-icon-color: $off-canvas-link-color;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Offcanvas section.

### Introduced classes

  - c-offcanvas\_\_user-area
  - c-offcanvas\_\_content
  - c-offcanvas--secondary

## HTML

``` 
<div class="off-canvas-wrap" id="js-main-offcanvas" data-offcanvas>
    <div class="inner-wrap">
      <aside class="left-off-canvas-menu">
        <ul class="off-canvas-list">
          <li class="c-offcanvas__user-area{% if not ses.profile.sesUser.isAnonymous %} is-logged-in{% endif %}">
            <h3 class="c-offcanvas__header">
              {{ 'My Account'|st_translate }}
                {% block language_switcher_mobile %}
                  <button href="#" data-dropdown="dropdown-languages-mobile" aria-controls="dropdown-languages-mobile"
                          aria-expanded="false" class="label secondary right">
                    {% if current_siteaccess != 'engl' %}DE{% else %}EN{% endif %}
                  </button>
                  <ul id="dropdown-languages-mobile" data-dropdown-content class="f-dropdown tiny" aria-hidden="true">
                    <li><a href="{{ url('silversolutions_language_switcher', {'siteaccess' : 'ger', 'source' : source, 'id' : id }) }}">DE</a></li>
                    <li><a href="{{ url('silversolutions_language_switcher', {'siteaccess' : 'engl', 'source' : source, 'id' : id }) }}">EN</a></li>
                  </ul>
                {% endblock %}
              {% if not ses.profile.sesUser.isAnonymous %}
                <div class="u-text-smaller">
                  {{ 'Logged in'|st_translate }}
                  {% if ses.profile.sesUser.contact|default is not empty and ses.profile.sesUser.contact.isBlocked %}
                    <span class="label alert">{{ 'blocked'|st_translate }}
                  {% endif %}
                
              {% endif %}
            </h3>
            <div class="c-offcanvas__content">
              {{ render_esi(
              controller(
              'SilversolutionsEshopBundle:CustomerProfileData:showHeaderLogin',
              {'isOffCanvas': true}
              )
              ) }}
            
          </li>
          <li>
            <label>{{ 'Main menu' }}</label>
          </li>
          {{ include('SilversolutionsEshopBundle:Catalog/Navigation:offcanvas.html.twig'|st_resolve_template) }}
          <li class="c-offcanvas--secondary">
            <label>{{ 'Service'|st_translate }}</label>
            {{ 'footer_block_company'|st_translate }}
            {{ 'footer_block_service'|st_translate }}
            {{ 'footer_block_ordering'|st_translate }}
          </li>
        </ul>
      </aside>
      <section class="main-section js-main-section">
        ....
      </section>
      <a class="exit-off-canvas"></a>

```
