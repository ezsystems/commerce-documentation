#  Navigation - Templates 

### How to render the navigation?

**Left navigation with product catalog as root element**

``` 
{# get the parent product catalog of the current catalog element #}
{% set product_catalog_id = get_parent_product_catalog(catalogElement) %}

{% include 'SilversolutionsEshopBundle:Navigation:left_navigation.html.twig'
  with { 'locationId': product_catalog_id}
%}

{# if you want, you can pass the id of the active catalog element, but it is not required, since the active node will be highlited automatically via js #}
{# if you pass th eactive node id, the navigation will be cached for this id #}
{% include 'SilversolutionsEshopBundle:Navigation:left_navigation.html.twig'
  with { 'locationId': product_catalog_id, 'active_node': catalogElement.id}
%}
```

**Left navigation that renders only part of the catalog (e.g. subcategories of the current catalog element)**

``` 
{# get the parent product catalog of the current catalog element #}
{% set product_catalog_id = get_parent_product_catalog(catalogElement) %}

{# even if you want to display only part of the catalog, the root node still must be the parent product catalog! #}
{% include 'SilversolutionsEshopBundle:Navigation:left_navigation.html.twig'
  with {'locationId': product_catalog_id, 'catalog_subtree': catalogElement.identifier}
%}
```

**Main navigation**

``` 
{% include 'SilversolutionsEshopBundle:Navigation:main_navigation.html.twig'|st_resolve_template
  with {'locationId':ses_config_parameter('navigation_ez_location_root', 'siso_core')}
%}
```

**Offcanvas navigation**

``` 
{% include 'SilversolutionsEshopBundle:Navigation:offcanvas_navigation.html.twig'|st_resolve_template
with {'locationId':ses_config_parameter('navigation_ez_location_root', 'siso_core')}
%}
```

### Templates list:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Path</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><p><strong>Main navigation  </strong>  </p></td>
<td><pre><code>*******************************************************</code></pre></td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/main_navigation.html.twig</code></pre></td>
<td>Renders the navigation subcontroller, that builds the main navigation</td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/main_menu.html.twig</code></pre></td>
<td>Renders the main navigation list &lt;ul&gt; and includes
<pre><code>knp_menu.html.twig</code></pre></td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/knp_menu.html.twig</code></pre></td>
<td>Renders the main navigation nodes as &lt;li&gt; elements</td>
</tr>
<tr>
<td><strong>Left navigation</strong></td>
<td><pre><code>*******************************************************</code></pre></td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/left_navigation.html.twig</code></pre></td>
<td>Renders the navigation subcontroller, that builds the left navigation</td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/left_menu.html.twig</code></pre></td>
<td>Renders the left navigation list &lt;ul&gt; and includes
<pre><code>left_menu.html.twig</code></pre></td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/knp_left_menu.html.twig</code></pre></td>
<td>Renders the left navigation nodes as &lt;li&gt; elements</td>
</tr>
<tr>
<td><strong>Offcanvas</strong></td>
<td><pre><code>*******************************************************</code></pre></td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/offcanvas_navigation.html.twig</code></pre></td>
<td>Renders the navigation subcontroller, that builds the offcanvas navigation</td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/offcanvas_menu.html.twig</code></pre></td>
<td>Renders the offcanvas navigation list &lt;ul&gt; and includes
<pre><code>knp_offcanvas.html.twig</code></pre></td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/knp_offcanvas.html.twig</code></pre></td>
<td>Renders the offcanvas navigation nodes as &lt;li&gt; elements</td>
</tr>
</tbody>
</table>

### Related custom Twig modifiers/functions/etc/:

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Function</th>
<th>Usage</th>
<th>Parameters</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>get_parent_product_catalog</code></pre></td>
<td>

<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{% set product_catalog_id = get_parent_product_catalog(catalogElement) %}</code></pre>
<p> </p>
<p>  </p></td>
<td> 
<pre><code>\Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement</code></pre></td>
<td><pre><code>Fetches the parent product catalog element for the $identifier</code></pre></td>
</tr>
<tr>
<td><pre><code>get_data_location_ids</code></pre></td>
<td>

<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{% set data_locations = get_data_location_ids(location) %}

{% set data_locations = get_data_location_ids(catalogElement) %}</code></pre>
<p><br />
</p></td>
<td><pre><code>\eZ\Publish\API\Repository\Values\Content\Location</code></pre>
<pre><code>or</code></pre>
<pre><code>\Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement</code></pre></td>
<td><pre><code>Returns a comma separated string of data_locations of the given element.</code></pre>
<pre><code>All parent locations incl. the element location are returned. </code></pre>
<pre><code>These data_locations are used to highlite the active node in the navigation. </code></pre>
<pre><code> </code></pre></td>
</tr>
</tbody>
</table>
