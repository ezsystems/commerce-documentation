# Language switcher

If your shop supports multiple language SiteAccesses, you can use the built-in language switcher function.

If a user clicks on a language in the switcher list, they are redirected to the same page in the appropriate language.

![](img/language_switcher_1.png)

## Configuration

SiteAccesses and languages are configured in `ezplatform.yml`:

``` yaml
ezpublish:
    siteaccess:
        default_siteaccess: ger
        list:            
            - engl
            - ger
            ...

    ...
    system:     
        engl:
            languages:
                - eng-US
       ger:
            languages:
                - ger-DE
```

## Template

The language switcher is integrated in `pagelayout.html.twig`, so it can be displayed on every page.

!!! note

    The parameters for the language switcher controller are set here. Do not change them to ensure the full functionality.

``` html+twig
{#parameters for the language switcher#}
{% set current_siteaccess = ezpublish.siteaccess.name %}
{% set id = '' %}
{% set source = '' %}
{% if location.id is defined %}
    {% set id = location.id  %}
    {% set source = 'eZ' %}
{% elseif catalogElement.identifier is defined %}
    {% set id = catalogElement.identifier %}
    {% set source = 'ssl' %}
{% elseif app.request.attributes.has('silverModule') %}
    {% set id = app.request.attributes.get('silverModule').versionInfo.contentInfo.id %}
    {% set source = 'silverModule' %}
{% else %}
    {% set source = 'url' %}
    {% set id = ezpublish.requestedUriString %}
{% endif %}
{#end - parameters for the language switcher#}
```

The list of languages is configured in these blocks. You have to change them depending on your SiteAccess configuration.

You need to set the correct SiteAccess name per language.

``` html+twig
{% block language_switcher_mobile %}

  <button href="#" data-dropdown="dropdown-languages-mobile" aria-controls="dropdown-languages-mobile"
          aria-expanded="false" class="label secondary right">
    {% if current_siteaccess != 'engl' %}DE{% else %}EN{% endif %}
  </button>

  <ul id="dropdown-languages-mobile" data-dropdown-content class="f-dropdown tiny" aria-hidden="true">
    <li><a href="{{ url('silversolutions_language_switcher', {'siteaccess' : 'ger', 'source' : source, 'id' : id, 'query_string' : app.request.queryString }) }}">DE</a></li>
    <li><a href="{{ url('silversolutions_language_switcher', {'siteaccess' : 'engl', 'source' : source, 'id' : id, 'query_string' : app.request.queryString }) }}">EN</a></li>
  </ul>

{% endblock %}

{% block language_switcher %}
      <li>
        <a href="#" class="c-nav-meta__dropdown with-arrow" data-dropdown="dropdown-languages" data-options="is_hover:true">
          <i class="fa fa-language"></i>
          {% if current_siteaccess != 'engl' %}Deutsch{% else %}English{% endif %}
        </a>

        <ul id="dropdown-languages" class="f-dropdown tiny text-left content c-nav-meta__content"
            data-dropdown-content>
          <li class="current">
            <a href="{{ url('silversolutions_language_switcher', {'siteaccess' : 'ger', 'source' : source, 'id' : id, 'query_string' : app.request.queryString }) }}">
              Deutsch
            </a>
          </li>
          <li>
            <a href="{{ url('silversolutions_language_switcher', {'siteaccess' : 'engl', 'source' : source, 'id' : id, 'query_string' : app.request.queryString }) }}">
              English
            </a>
          </li>
        </ul>
      </li>
{% endblock %}
```

## Controller

`LanguageSwitcherController` generates the correct URL depending on the given GET parameters in the template and redirects the user to the correct page.
If no URL can be generated, the user is redirected to the homepage of the requested SiteAccess.

``` yaml
silversolutions_language_switcher:
    path:     /language_switcher
    defaults: { _controller: SilversolutionsEshopBundle:LanguageSwitcher:redirect } 
```
