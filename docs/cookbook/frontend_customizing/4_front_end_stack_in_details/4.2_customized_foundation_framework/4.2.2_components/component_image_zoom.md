#  Component - Image Zoom 

This section describes image zoom functionality which is commonly used on e-commerce websites to show details on images. Storm is using plugin called xZoom to achieve image zoom functionalities.

This plugin is open source 

 <https://github.com/payalord/xZoom>

  - Supports jQuery starting from version 1.2.6.
  - A lof of options, effects and easy to use and customize
  - Lightweight \~14kb minified version.
  - You can load low and high res images separately.
  - Supports IE6+, Chrome, FireFox, Opera, Safari, Android, iOS
  - Supports Responsive output.
  - Have an API to integrate with other useful plugins like [FancyBox](http://www.fancyapps.com/fancybox/), [Magnific PopUp](http://dimsemenov.com/plugins/magnific-popup/) and [HammerJS](http://hammerjs.github.io/).

## Sass / CSS

Currently there is no sass provided. We just import standard CSS file in our scss/style.scss file in the Pugins section. The file is located here:

**File location**

``` 
vendor/xzoom/xzoom.css
```

## Quickstart

## HTML

### Step 1:

1.  Copy `xzoom.min.js` or `xzoom.js` file into your javascript folder.
2.  Copy `xzoom.css` file into your css folder, or copy the content of the `xzoom.css` file into your site style sheet.
3.  Copy `example/images/xloading.gif` to your images folder.

### Step 2:

This goes into your site's Header Section:

``` 
<!-- get jQuery from the google apis or use your own -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>

<!-- CSS STYLE-->
<link rel="stylesheet" type="text/css" href="css/xzoom.css" media="all" />

<!-- XZOOM JQUERY PLUGIN  -->
<script type="text/javascript" src="js/xzoom.min.js"></script>
```

### Step 3:

Add xZoom markup into your HTML:

``` 
<img class="xzoom" src="path/to/preview_image_01.jpg" xoriginal="path/to/original_image_01.jpg" />

<div class="xzoom-thumbs">
  <a href="path/to/original_image_01.jpg">
    <img class="xzoom-gallery" width="80" src="path/to/thumbs_image_01.jpg"  xpreview="path/to/preview_image_01.jpg">
  </a>
  <a href="path/to/original_image_02.jpg">
    <img class="xzoom-gallery" width="80" src="path/to/preview_image_02.jpg">
  </a>
  <a href="path/to/original_image_03.jpg">
    <img class="xzoom-gallery" width="80" src="path/to/preview_image_03.jpg">
  </a>
  <a href="path/to/original_image_04.jpg">
    <img class="xzoom-gallery" width="80" src="path/to/preview_image_04.jpg">
  </a>

```

### Step 4:

Initialize the plugin in "document ready" section of your javascript or at the end before `</body>`:

``` 
/* calling script */
$(".xzoom, .xzoom-gallery").xzoom({tint: '#333', Xoffset: 15});
```

### Configuration

Full list of available properties can be found on the plugins website: <http://www.starplugins.com/cloudzoom/quickstart>

| Property        | Default       | Description                                                                                                                                                          |
| --------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| position        | right         | Position of zoom output window, one of the next properties is available "top", "left", "right", "bottom", "inside", "lens", "\#ID".                                  |
| mposition       | inside        | Position of zoom output window in adaptive mode (i.e. for mobile devices) available properties: "inside", "fullscreen"                                               |
| rootOutput      | true          | In the HTML structure, this option gives an ability to output xzoom element, to the end of the document body or relative to the parent element of main source image. |
| Xoffset         | 0             | Zoom output window horizontal offset in pixels from output base position.                                                                                            |
| Yoffset         | 0             | Zoom output window vertical offset in pixels from output base position.                                                                                              |
| fadeIn          | true          | Fade in effect, when zoom is opening.                                                                                                                                |
| fadeTrans       | true          | Fade transition effect, when switching images by clicking on thumbnails.                                                                                             |
| fadeOut         | false         | Fade out effect, when zoom is closing.                                                                                                                               |
| smoothZoomMove  | 3             | Smooth move effect of the big zoomed image in the zoom output window. The higher value will make movement smoother.                                                  |
| smoothLensMove  | 1             | Smooth move effect of lens.                                                                                                                                          |
| smoothScale     | 6             | Smooth move effect of scale.                                                                                                                                         |
| defaultScale    | 0             | You can setup default scale value of zoom on opening, from -1 to 1. Where -1 means -100%, and 1 means 100% of lens scale.                                            |
| scroll          | true          | Scale on mouse scroll.                                                                                                                                               |
| tint            | false         | Tint color. Color must be provided in format like "\#color". We are not recommend you to use named css colors.                                                       |
| tintOpacity     | 0.5           | Tint opacity from 0 to 1.                                                                                                                                            |
| lens            | false         | Lens color. Color must be provided in format like "\#color". We are not recommend you to use named css colors.                                                       |
| lensOpacity     | 0.5           | Lens opacity from 0 to 1.                                                                                                                                            |
| lensShape       | box           | Lens shape "box" or "circle".                                                                                                                                        |
| lensCollision   | true          | Lens will collide and not go out of main image borders. This option is always false for position "lens".                                                             |
| lensReverse     | false         | When selected position "inside" and this option is set to true, the lens direction of moving will be reversed.                                                       |
| openOnSmall     | true          | Option to control whether to open or not the zoom on original image, that is smaller than preview.                                                                   |
| zoomWidth       | auto          | Custom width of zoom window in pixels.                                                                                                                               |
| zoomHeight      | auto          | Custom height of zoom window in pixels.                                                                                                                              |
| sourceClass     | xzoom-source  | Class name for source "div" container.                                                                                                                               |
| loadingClass    | xzoom-loading | Class name for loading "div" container that appear before zoom opens, when image is still loading.                                                                   |
| lensClass       | xzoom-lens    | Class name for lens "div".                                                                                                                                           |
| zoomClass       | xzoom-preview | Class name for zoom window(div).                                                                                                                                     |
| activeClass     | xactive       | Class name that will be added to active thumbnail image.                                                                                                             |
| hover           | false         | With this option you can make a selection action on thumbnail by hover mouse point on it.                                                                            |
| adaptive        | true          | Adaptive functionality.                                                                                                                                              |
| adaptiveReverse | false         | Same as lensReverse, but only available when adaptive is true.                                                                                                       |
| title           | false         | Output title/caption of the image, in the zoom output window.                                                                                                        |
| titleClass      | xzoom-caption | Class name for caption "div" container.                                                                                                                              |
| bg              | false         | Zoom image output as background, works only when position is set to "lens".                                                                                          |

You can find the whole manual here: <https://github.com/payalord/xZoom/blob/master/doc/manual.md>

## This example comes from product detail page using partial template. It combines an image zoom with slider for thumbnails.  
Twig

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/productDetail.html.twig**

``` 
{% set mainImage = null %}
{% set productId = null %}
{% set imageListWithoutMain = null %}

<div class="row js-variant-content-wrapper">
    <div class="medium-12 large-6 columns print-product-detail__img">

      {# TODO: how to handle video link #}
      {% if ses_render_field(catalogElement, 'video') %}
          <a class="video_link" href="{{ ses_render_field(catalogElement, 'video') }}">
              <span class="sprite sprite-032b-video-30 trans">
          </a>
      {% endif %}

      {% if orderableVariantProduct|default is not empty %}
        {% set productId = catalogElement.sku ~ orderableVariantProduct.variantCode %}
        {% set mainImage = ses_assets_main_image(orderableVariantProduct, productId)  %}
      {% endif %}
      {% if mainImage is empty %}
          {% set mainImage = ses_assets_main_image(catalogElement)  %}
      {% endif %}
      <img class="xzoom" src="/{{ st_imageconverter(mainImage, 'thumb_big') }}" xoriginal="/{{ st_imageconverter(mainImage, 'image_zoom') }}" />

      <div class="thumbs u-margin-top-1x js-slick-init xzoom-thumbs" data-slick='{"slidesToShow": 4, "slidesToScroll": 4}'>

        {% set mainImages = [mainImage] %}
        {% if orderableVariantProduct|default is not empty %}
            {% set productId = catalogElement.sku ~ orderableVariantProduct.variantCode %}
            {% set imageListWithoutMain = ses_assets_image_list(orderableVariantProduct, productId) %}
        {% endif %}

        {% if imageListWithoutMain is empty %}
            {% set imageListWithoutMain = ses_assets_image_list(catalogElement, productId) %}
        {% endif %}

        {% set imageList = mainImages|merge(imageListWithoutMain) %}
        {% if imageList|length > 0 %}
              {% for item in imageList %}

              <a href="/{{ st_imageconverter(item, 'image_zoom') }}">
                <img class="xzoom-gallery" src="/{{ st_imageconverter(item, 'thumb_smaller') }}"  xpreview="/{{ st_imageconverter(item, 'thumb_big') }}">
              </a>

              {% endfor %}
          {% endif %}

    <div class="medium-12 large-6 columns print-product-detail__data">
        {{ include('SilversolutionsEshopBundle:Catalog:parts/productData.html.twig'|st_resolve_template) }}
        {% block product_variant %}{% endblock %}

```
