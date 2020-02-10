#  RoutesBreadcrumbsGenerator 

This generates the breadcrumbs for the symfony routes.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td>FQN</td>
<td>Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\RoutesBreadcrumbsGenerator</td>
</tr>
<tr>
<td>Criterion for responsibility</td>
<td>If the eZ\Publish\Core\MVC\Symfony\Routing\ChainRouter matches the request, this generator is responsible. Since this is the case for nearly every request, this generator must be registered with (one of) the lowest priority(/ies).</td>
</tr>
<tr>
<td>Notes to the rendering process</td>
<td><p>Renders breadcrumbs, if the route for the currently matched controller action contains additional information for its the breadcrumbs (routing.yml):</p>
<strong>Example</strong>
<pre class="" data-syntaxhighlighter-params="brush: text; gutter: false; theme: Confluence" data-theme="Confluence"><code>siso_global_search:
    path: /search_query
    defaults:
        _controller: SisoSearchBundle:Search:search
        breadcrumb_path: siso_global_search
        breadcrumb_names: Search
 </code></pre>
<ul>
<li><em><code>breadcrumb_path</code></em> defines a character ('/') separated sequence of route identifiers, which represents the links of the breadcrumbs elements.</li>
<li><code>breadcrumb_names</code> is a similar sequence of same order and number as <code>breadcrumb_path</code> and specifies the displayed names of the breadcrumb elements. The values are not rendered directly, but define the source for a <a href="Translations_23560284.html">translation</a>.</li>
</ul>
<p> </p></td>
</tr>
</tbody>
</table>

Limitation

There is one limitation when using the breadcrumb for the custom routes. When you want to restrict the HTTP method for the controller, you can not do it in the routing file.

``` 
siso_newsletter_subscribe:
    path:     /newsletter/subscribe
    defaults: 
        _controller: SisoNewsletterBundle:Newsletter:subscribeNewsletter
        breadcrumb_path: siso_newsletter_subscribe
        breadcrumb_names: subscribe newsletter
    methods: POST
```

If you do so, you will see incorrect breadcrumb.

To see the correct breadcrumb, you have to check the method in the controller itself

``` 
siso_newsletter_subscribe:
    path:     /newsletter/subscribe
    defaults: 
        _controller: SisoNewsletterBundle:Newsletter:subscribeNewsletter
        breadcrumb_path: siso_newsletter_subscribe
        breadcrumb_names: subscribe newsletter

...
if ($request->getMethod() != REQUEST::METHOD_POST) {
    throw new NotFoundHttpException();
}
```
