# Navigation Component

We use this component to set active class to the navigation elements. We use JavaScript for this in order to make caching easier and more performant. Using navigation component we are able to set the active class to any type of navigation. Currently we use to handle top navigation as well as sidebar navigation but there is no 

## Files

``` 
// JavaScript
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/components/navigation.js
```

### Available methods

#### storm.setActiveNav()

Runs on page load and set's active class to each item that belongs to the path.
Can be used to set the active class to any navigation element. Jus make sure to apply data-location-id to the element with a proper value.
Currently the script supports active class setup for .js-main-nav as well as .js-side-nav. Make sure the navigation you want to set active classes to is in the list of navigations.

```
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/components/navigation.js:11
var nav = ['.js-main-nav', '.js-side-nav'];
```

Nav variable is a JavaScript array with jQuery selectors that should be checked against the list of ids stored in the data attribute attached to the <body> element

```
storm.setActiveNav()
```

## HTML / Twig

The way how this component is able to work is to set data-locations-ids attribute to the \<body\> element. Each id must be separated by a comma.

### Twig

``` 
<body class="{% block main_css_class %}{% endblock %}" data-siteaccess="{{ '/'|st_siteaccess_path }}" data-phalanx-url="{{ path('silversolutions_phalanx') }}" data-location-ids="{{ data_locations }}">
```

### HTML (generated example)

#### Body element

``` 
<body class="js-is-search js-is-search-catalog" data-siteaccess="/" data-phalanx-url="/_ajax_/phalanx" data-location-ids="catalog_1,catalog_2,catalog_86,catalog_9686,catalog_10419,catalog_10420,ez_1,ez_2,ez_86">
```

#### Example link element:

``` 
// top navigation element
<li class="has-dropdown js-has-dropdown active not-click" data-location-id="ez_86"> .... </li>
 
// top navigation element
<li class="active" data-location-id="catalog_10420"> .... </li>
 
// sidebar navigation
<li class="has-dropdown js-has-dropdown active" data-location-id="catalog_10419" role="menuitem"> .... </li>
```
