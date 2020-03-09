# Component - Text buttons

Component used for "add to" features on product detail page in the sidebar (price block).

## Sass

**File location**

``` 
scss/storm/_components.text-buttons.scss
```

### Default settings:

``` 
$text-buttons-margin: 0;

$text-buttons-item-margin-bottom: rem-calc(5);
$text-buttons-item-padding-bottom: rem-calc(5);
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Text Buttons section.

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/productDetailButtonGroup.html.twig**

``` 
<ul class="c-text-buttons">
  {% if ses_check_product_in_wish_list(catalogElement.sku, variantCode) == false %}
    {% if ses.profile.sesUser.isAnonymous %}
      <li
        class="c-text-buttons__item is-disabled tip-left" data-tooltip aria-haspopup="true"
        data-options="disable_for_touch:true" title="{{ 'Please log in to use this feature'|st_translate }}">
        <a href="#" class="u-block u-text-small">
          <i class="fa fa-heart fa-lg fa-fw"></i> {{ 'Add to wishlist'|st_translate }}
        </a>
      </li>
    {% else %}
      <li class="c-text-buttons__item">
        <a href="#" class="u-block u-text-small js-add-to-wishlist" data-text-swap="{{ 'Already in your wishlist'|st_translate }}">
          <i class="fa fa-heart fa-lg fa-fw"></i> <span class="js-text-swap-wrapper">{{ 'Add to wishlist'|st_translate }}
        </a>
      </li>
    {% endif %}
  {% else %}
    <li class="c-text-buttons__item is-disabled">
      <a href="#" class="u-block u-text-small">
        <i class="fa fa-heart fa-lg fa-fw"></i> {{ 'Already in your wishlist'|st_translate }}
      </a>
    </li>
  {% endif %}

  {% if ses.profile.sesUser.isAnonymous %}
    <li
      class="c-text-buttons__item is-disabled tip-left" data-tooltip aria-haspopup="true"
      data-options="disable_for_touch:true" title="{{ 'Please log in to use this feature'|st_translate }}">
      <a href="#" class="u-block u-text-small">
        <i class="fa fa-upload fa-lg fa-fw"></i> {{ 'Add to stored basket'|st_translate }}
      </a>
    </li>
  {% else %}
    <li class="c-text-buttons__item">
      <a href="#" class="u-block u-text-small js-add-to-stored-basket">
        <i class="fa fa-upload fa-lg fa-fw"></i> {{ 'Add to stored basket'|st_translate }}
      </a>
    </li>
  {% endif %}

  {% if ses_check_product_in_comparison(catalogElement.sku, variantCode) == false %}
    <li class="c-text-buttons__item show-for-large-up">
      <a href="#" class="u-block u-text-small js-add-to-comparison"
         data-compare-category="{{ ses_comparison_category(catalogElement) }}"
         data-text-swap="{{ 'Already in your comparison list'|st_translate }}">
        <i class="fa fa-files-o fa-lg fa-fw"></i> <span class="js-text-swap-wrapper">{{ 'Compare'|st_translate }}
      </a>
    </li>
  {% else %}
    <li class="c-text-buttons__item show-for-large-up is-disabled">
      <a href="#" class="u-block u-text-small">
        <i class="fa fa-files-o fa-lg fa-fw"></i> {{ 'Already in your comparison list'|st_translate }}
      </a>
    </li>
  {% endif %}
</ul>
```
