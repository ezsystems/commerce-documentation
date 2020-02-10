#  Component - User Menu 

Component for a dropdown menu containing different functions related to logged in user.

## Sass

**File location**

``` 
scss/storm/_components.user-menu.scss
```

## Twig

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/parts/user\_menu.html.twig**

``` 
<a class="right u-float-left-on-small c-user-menu__trigger" data-dropdown="dropdown-user-menu" aria-controls="dropdown-user-menu" aria-expanded="false"
   data-options="is_hover: true;  hover_timeout: 0">
  <i class="fa fa-puzzle-piece fa-fw"></i>
  {{ 'User menu'|st_translate }}
  <i class="fa fa-caret-down"></i>
</a>
<div class="f-dropdown content c-user-menu__content" id="dropdown-user-menu"  data-dropdown-content aria-hidden="true" tabindex="-1">
  <ul class="no-bullet">
    {{ ses_user_menu()|raw }}
  </ul>

```

### Twig function

There is a  twig function, that dynamically includes these subtemplates (based on user permissions):

``` 
{{ ses_user_menu()|raw }} 
```

  - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Components/user\_menu.html.twig
  - vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/views/CustomerCenter/Components/user\_menu.html.twig
  - vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/user\_menu.html.twig

### Configuration

Use the configuration to define which bundles are used in order to extend the user menu.

``` 
siso_core.default.user_menu_bundles:
    - SilversolutionsEshopBundle
    - SisoCustomerCenterBundle
    - SisoOrderHistoryBundle
``` 
