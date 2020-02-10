#  Breadcrumbs - Templates 

### Templates list:

| Path                                                                                      | Description                                                                                                                                             |
| ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig                   | Defines the *`breadcrumb`* block, which calls the sub-controller for the breadcrumbs generation. This block could be overridden by extending templates. |
| Silversolutions/Bundle/EshopBundle/Resources/views/Breadcrumbs/breadcrumb\_list.html.twig | Template, which is used by all WhiteOctober based breadcrumbs generators, in order to render the generated breadcrumbs elements into HTML.              |

**Breadcrumbs/breadcrumb\_list.html.twig**

``` 
<div class="crumb-wrap hide-for-print">
  <div class="row">
    <div class="columns">
      {% if wo_breadcrumbs()|length %}
        <ul class="breadcrumbs u-no-margin show-for-large-up" itemscope itemtype="http://schema.org/BreadcrumbList">
          {% for crumb in breadcrumbs %}
            {% if not loop.last %}
              <li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
                <a itemprop="item" href="{{ crumb.url }}">
                  <span itemprop="name">{{ crumb.text }}
                </a>
                <meta itemprop="position" content="{{ loop.index }}" />
              </li>
            {% else %}
              {# TODO make last element link configurable #}
              <li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
                <span itemprop="name">{{ crumb.text }}
                <meta itemprop="position" content="{{ loop.index }}" />
              </li>
            {% endif %}
          {% endfor %}
        </ul>
      {% endif %}
```

### Related custom Twig modifiers/functions/etc/:

wo\_breadcrumbs() function is used in breadcrumb\_list.html.twig in order to determine the count of generated breadcrumbs. Furthermore the breadcrumbs variable, used in this template, is passed by the BreadcrumbsHelper.

###   
### Additional data in the breadcrumb nodes

"crumb.translationParameters" contains additional data which can be used to change the behavior of the breadcrumb depending on the stored data:

<table>
<thead>
<tr class="header">
<th>CatalogBreadcrumbsGenerator.php<br />
</th>
<th>EzContentBreadcrumbsGenerator.php</th>
<th>RoutesBreadcrumbsGenerator.php</th>
</tr>
</thead>
<tbody>
<tr>
<td>

<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&#39;TYPE&#39; =&gt; STRING &#39;CATALOG&#39; (LENGTH=7)
&#39;IDENTIFIER&#39; =&gt; STRING &#39;9686&#39; (LENGTH=4)
&#39;CONTENT_TYPE_ID&#39; =&gt; STRING &#39;&#39; (LENGTH=0)</code></pre>
<div class="content-wrapper">
<pre><code></code></pre>
<p><br />
</p>
</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&#39;TYPE&#39; =&gt; STRING &#39;EZ_CONTENT&#39; (LENGTH=10)
&#39;IDENTIFIER&#39; =&gt; INT 57
&#39;CONTENT_TYPE_ID&#39; =&gt; INT 23</code></pre>
<br />

<p><br />
</p>
</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&#39;TYPE&#39; =&gt; STRING &#39;ROUTE&#39; (LENGTH=5)
&#39;IDENTIFIER&#39; =&gt; STRING &#39;&#39; (LENGTH=0)
&#39;CONTENT_TYPE_ID&#39; =&gt; STRING &#39;&#39; (LENGTH=0)</code></pre>
<br />

<p><br />
</p>
</td>
</tr>
</tbody>
</table>

Example for not clickable catalog breadcrumb in template:

**breadcrumb\_list.html.twig**

``` 
{% if crumb.translationParameters.type == 'catalog' %}
  <span itemprop="name">{{ crumb.text }}
  <meta itemprop="position" content="{{ loop.index }}" />
{% else %}
  <a itemprop="item" href="{{ crumb.url }}">
    <span itemprop="name">{{ crumb.text }}
  </a>
  <meta itemprop="position" content="{{ loop.index }}" />
{% endif %}
```
