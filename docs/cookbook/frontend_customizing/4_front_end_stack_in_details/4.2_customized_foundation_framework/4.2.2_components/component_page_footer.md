#  Component - Page Footer 

Component used for the page footer content. We are using text modules created in eZ Platform to include the specific content.

  - footer\_block\_address
  - footer\_block\_company
  - footer\_block\_service
  - footer\_block\_ordering

## Sass

**File location**

``` 
scss/storm/_components.page-footer.scss
```

### Default settings:

``` 
$page-footer-margin-top: rem-calc(25);
$page-footer-padding: rem-calc($rem-base) 0;

$page-footer-background: lighten($flat-silver, 18%);

$page-footer-font-size: rem-calc(14)

$page-footer-column-border-right-width: rem-calc(1);
$page-footer-column-border-right-style: solid;
$page-footer-column-border-right-color: $gainsboro;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Page Footer section.

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig**

``` 
<div class="c-page-footer u-margin-top-4x js-footer-wrapper">

  <div class="row" data-equalizer>
    <div class="large-3 columns c-page-footer__column" data-equalizer-watch>
      {{ 'footer_block_address'|st_translate }}
    
    <div class="large-3 columns c-page-footer__column" data-equalizer-watch>
      {{ 'footer_block_company'|st_translate }}
    
    <div class="large-3 columns c-page-footer__column hide-for-small" data-equalizer-watch>
      {{ 'footer_block_service'|st_translate }}
    
    <div class="large-3 columns c-page-footer__column hide-for-small">
      {{ 'footer_block_ordering'|st_translate }}

  <div class="row">
    <div class="columns">
      <hr/>
    
    <div class="small-12 medium-6 columns">
      <strong>{{ 'Social media'|st_translate }}:</strong>
      <a href="http://www.twitter.com/silver_berlin">
        <i class="fa fa-twitter fa-lg fa-fw"></i>
      </a>
      <a href="https://www.xing.com/companies/silver.solutionsgmbh/employees">
        <i class="fa fa-xing fa-lg fa-fw"></i>
      </a>
    
    <div class="small-12 medium-6 columns u-margin-top-1x-on-small medium-text-right">
      <strong>{{ 'Flexible payment options'|st_translate }}:</strong>
      <i class="fa fa-paypal fa-lg fa-fw"></i>
      <i class="fa fa-cc-visa fa-lg fa-fw"></i>
      <i class="fa fa-cc-mastercard fa-lg fa-fw"></i>
      <i class="fa fa-cc-amex fa-lg fa-fw"></i>

  <div class="row">
    <div class="column">
      <hr/>
    
    <div class="column">
        <ul class="c-tagcloud">
            <li class="c-tagcloud__item"><strong>{{ 'Popular search terms'|st_translate }}:</strong></li>
            <li class="c-tagcloud__item"><a href="/Produkt-Katalog/Lautsprecher">Speakers</a></li>
            <li class="c-tagcloud__item">
                <br/>{{ 'Discover more'|st_translate }}:
                <a href="/tags/a">A</a> <a href="/tags/b">B</a> <a href="/tags/c">C</a> <a href="/tags/d">D</a> <a
                        href="/tags/e">E</a> <a href="/tags/f">F</a> <a href="/tags/g">G</a> <a href="/tags/h">H</a> <a
                        href="/tags/i">I</a> <a href="/tags/j">J</a> <a href="/tags/k">K</a> <a href="/tags/l">L</a> <a
                        href="/tags/m">M</a> <a href="/tags/n">N</a> <a href="/tags/o">O</a> <a href="/tags/p">P</a> <a
                        href="/tags/q">Q</a> <a href="/tags/r">R</a> <a href="/tags/s">S</a> <a href="/tags/t">T</a> <a
                        href="/tags/u">U</a> <a href="/tags/v">V</a> <a href="/tags/w">W</a> <a href="/tags/x">X</a> <a
                        href="/tags/y">Y</a> <a href="/tags/z">Z</a> <a href="/tags/0">0</a> <a href="/tags/1">1</a> <a
                        href="/tags/2">2</a> <a href="/tags/3">3</a> <a href="/tags/4">4</a> <a href="/tags/5">5</a> <a
                        href="/tags/6">6</a> <a href="/tags/7">7</a> <a href="/tags/8">8</a> <a href="/tags/9">9</a>
            </li>
        </ul>
    
    <div class="column">
      <hr/>
    
    <div class="column">
      &copy; {{ "now"|date("Y") }} silver.solutions GmbH
      {# no sense to show cookie message to logged in users since login state is kept in cookie anyway #}
      {% if ses.profile.sesUser.isAnonymous %}<span class="right cookie_link" data-policy-link="">{% endif %}

```
