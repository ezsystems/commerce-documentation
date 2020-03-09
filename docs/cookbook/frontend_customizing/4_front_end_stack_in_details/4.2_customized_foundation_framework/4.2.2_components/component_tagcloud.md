# Component - Tagcloud

We use this component to style links inside a tag cloud.

## Sass

**File location**

``` 
scss/storm/_components.tagcloud.scss
```

### Default settings:

``` 
$tagcloud-margin: rem-calc(0);
$tagcloud-font-size: rem-calc(12);
$tagcloud-separator-icon: "\00B7";
$tagcloud-separator-margin-right: rem-calc(5);
$tagcloud-item-display: inline;
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Tagcloud section.

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig**

``` 
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
```
