#  Non-blocking notifications aka toastr

Toastr is a non-blocking notification. Non-blocking means you can still use the page, while something is being done behind the scenes (e.g. add to basket, add to comparison, etc.).

Our non-blocking notifications are build on top of jQuery Toastr plugin:

  - <http://codeseven.github.io/toastr/> - project homepage
  - <http://codeseven.github.io/toastr/demo.html> - demos playground

## JavaScript:

**File location**

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/phalanx/storm.phalanx.utils.js
```

Phalanx utility file contains couple of helpers. Tostr is one of them.

### Default configuration:

We use these settings as a default configuration: 

``` 
// global configuration
toastr.options = {
  positionClass: "toast-top-right",
  showMethod: 'slideDown',
  hideMethod: 'slideUp',
  closeMethod: 'slideUp',
  timeOut: 5000,
  closeButton: true,
  tapToDismiss: false,
  target: Foundation.utils.is_large_up() ? '.js-toastr-target' : 'body'
};
```

If you want to change them we suggest to copy storm.phalanx.utils.js into your project structure and modify as you wish. For possible configuration options please visit project's homepage at: <http://codeseven.github.io/toastr/> 

### Available methods:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Description</th>
<th>Examples</th>
</tr>
</thead>
<tbody>
<tr>
<td>storm.toastr(msg, type, options)</td>
<td><p>Opens a toastr message. It takes three parameters:</p>
<ul>
<li>msg - message content. HTML allowed</li>
<li>type - type of message. Based on this toastr appears in different color schemes. Possible values: success, error, info, warning</li>
<li>options - it's possible to override default options per message using a obejct notation</li>
</ul></td>
<td>

<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>// standard success message
var message = &#39;success messages&#39;;
storm.toastr(message, &#39;success&#39;);
 
// standard error message
var message = &#39;error messages&#39;;
storm.toastr(message, &#39;error&#39;);
 
// info message with some options
// toastr will show with a slideUp method and stay visible for 10 seconds
var options = { timeOut: 10000, showMethod: &#39;slideUp&#39; }
var message = &#39;info messages&#39;;
storm.toastr(message, &#39;info&#39;, options);</code></pre>

</td>
</tr>
</tbody>
</table>

## Sass:

**File location**

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/storm/_extend.components.toastr.scss
```

Our Sass implemention is just an extension what's in the standard CSS file. On top of the standard configuration we put our colors and font styling. 

If you want to override it in your project please copy it to your project destination and make some modifications as you wish.

### Default settings:

``` 
$toastr-base-border-radius: $alert-radius;
$toastr-base-border-left-width: $alert-left-border-width;
$toastr-base-border-left-style: solid;

$toastr-box-shadow-bg: $base;

$toastr-icon-darken: 10%;

$toastr-container-xxlarge-left: 50%;
$toastr-container-xxlarge-margin-left: rem-calc(275);

$toastr-container-inner-large-width: rem-calc(300);
$toastr-container-inner-margin: rem-calc(0 0 10);
$toastr-container-inner-padding: rem-calc(15 25 15 50);
$toastr-container-inner-font-size: rem-calc(14);
$toastr-container-inner-liner-height: normal;
$toastr-container-inner-border-radius: 0;
$toastr-container-inner-box-shadow: 0 0 30px $toastr-box-shadow-bg;
$toastr-container-inner-box-shadow-hover: 0 0 30px darken($toastr-box-shadow-bg, 5%);
$toastr-container-inner-opacity: 1;

$toastr-container-icon-position: absolute;
$toastr-container-icon-left: rem-calc(15);
$toastr-container-icon-top: 50%;
$toastr-container-icon-font-family: FontAwesome;
$toastr-container-icon-font-size: rem-calc(24);
$toastr-container-icon-transform: translateY(-50%);

$toastr-success-color: darken($success-color, $alert-text-color-darken);
$toastr-success-bg-color: $alert-success-bg-color;
$toastr-success-bg-image: none !important;
$toastr-success-icon-symbol: '\f058';
$toastr-success-icon-color: darken($success-color, $toastr-icon-darken);

$toastr-alert-color: darken($alert-color, $alert-text-color-darken);
$toastr-alert-bg-color: $alert-alert-bg-color;
$toastr-alert-bg-image: none !important;
$toastr-alert-icon-symbol: '\f06a';
$toastr-alert-icon-color: darken($alert-color, $toastr-icon-darken);

$toastr-info-color: darken($info-color, $alert-text-color-darken);
$toastr-info-bg-color: $alert-info-bg-color;
$toastr-info-bg-image: none !important;
$toastr-info-icon-symbol: '\f05a';
$toastr-info-icon-color: darken($info-color, $toastr-icon-darken);

$toastr-warning-color: darken($warning-color, $alert-text-color-darken);
$toastr-warning-bg-color: $alert-warning-bg-color;
$toastr-warning-bg-image: none !important;
$toastr-warning-icon-symbol: '\f071';
$toastr-warning-icon-color: darken($warning-color, $toastr-icon-darken);

$toastr-close-top: rem-calc(16) !important;
$toastr-close-right: rem-calc(6) !important;
$toastr-close-font-weight: normal;
$toastr-close-text-shadow: none;
$toastr-close-opacity: 0.3;
$toastr-close-hover-bg: none;
$toastr-close-hover-opacity: 0.5;
```
