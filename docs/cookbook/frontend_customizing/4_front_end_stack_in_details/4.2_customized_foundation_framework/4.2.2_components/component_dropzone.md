# Component - Dropzone

We use dropzone plugin in quick order module. It allows to use drag\&drop functionality. Dropzone is using <https://github.com/blueimp/jQuery-File-Upload>. 

## Sass

**File location**

``` 
scss/storm/_components.dropzone.scss
```

### Default settings:

``` 
$dropzone-height: rem-calc(249);
$dropzone-padding: rem-calc(60);
$dropzone-line-height: rem-calc(32);
$dropzone-text-align: center;
$dropzone-bg-color: $flat-clouds;
$dropzone-border-width: rem-calc(2);
$dropzone-border-style: dashed;
$dropzone-border-color: $flat-silver;

$dropzone-plus-height: rem-calc(60);
$dropzone-plus-width: $dropzone-plus-height;
$dropzone-plus-line-height: $dropzone-plus-height;
$dropzone-plus-margin: rem-calc(0) auto;
$dropzone-plus-border-width: $dropzone-border-width;
$dropzone-plus-border-style: $dropzone-border-style;
$dropzone-plus-border-color: $dropzone-border-color;
$dropzone-plus-icon: $fa-var-upload; // please FontAwesome values, or override the selector in your project
$dropzone-plus-icon-font-size: rem-calc(24);
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Dropzone section.

## Vendor with dependencies

``` 
vendor/blueimp-file-upload
-- vendor/blueimp-canvas-to-blob
-- vendor/blueimp-load-image
-- vendor/blueimp-tmpl
```

## Twig

``` 
<div id="dropzone" class="fade well">
  <div class="plus">
    <i class="fa fa-upload fa-lg"></i>
  
  {{ 'Drop file here or'|st_translate }}
  {# add class hide to hide the button #}
  <input id="fileupload" type="file" name="files[]" data-url="{{ path('siso_quick_order_upload') }}"/>
```

We use Font Awesome to display an icon inside the dropzone.
