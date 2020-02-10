#  Breadcrumbs - API 

### How do breadcrumbs work?

#### Page layout hook-in

The template *Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig* defines a block named `breadcrumb` in which the breadcrumbs are rendered as a sub-controller call.

The controller's short name is SilversolutionsEshopBundle:Breadcrumbs:renderBreadcrumbs.

FQN: `\Silversolutions\Bundle\EshopBundle\Controller\BreadcrumbsController::renderBreadcrumbsAction()`

#### Aggregation of generators

Breadcrumbs logic is using aggregate method for each generator interface.  The UML diagram is shown below.

![](attachments/23560525/23563402.png)

The main gateway is a class "BreadcrumbsAggregateGenerator". It is collecting all generators, which can generate breadcrumbs. 

We use a compiler pass to get all services tagged with "siso\_core.breadcrumbs\_generator".

``` 
<tag name="siso_core.breadcrumbs_generator" priority="20" />
```

#### Rendering of breadcrumbs

The controller method BreadcrumbsController::renderBreadcrumbsAction() uses the BreadcrumbsAggregateGenerator to render the breadcrumbs from the controller.

The AggregateGenerator loops through all collected generators to check if this particular generator can render breadcrumbs.

The first generator that returns true from canRenderBreadcrumbs() wins and will be used to render the breadcrumbs HTML code. That's why it is important to set the priority of the generators properly.

Every generator needs to implement the "Silversolutions\\Bundle\\EshopBundle\\Api\\BreadcrumbsGeneratorInterface".

<table>
<thead>
<tr class="header">
<th>Method</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>public function canRenderBreadcrumbs(Request $request);</code></pre></td>
<td><p>This method verifies if the generator should render breadcrumbs for current request.</p></td>
</tr>
<tr>
<td><pre><code>public function renderBreadcrumbs(Request $request);</code></pre></td>
<td><p>This method is responsible for the breadcrumbs generation.</p>
<p>It renders the breadcrumbs for the current request.</p></td>
</tr>
</tbody>
</table>

#### List of generators:

<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><p><a href="RoutesBreadcrumbsGenerator_23560748.html">RoutesBreadcrumbsGenerator</a></p></td>
<td><div class="content-wrapper">
<p>renders breadcrumbs, if the route for the action contains information (routing.yml):</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>breadcrumb_path: silversolutions_stored_basket_show
breadcrumb_names: storedBasket</code></pre>
</td>
</tr>
<tr>
<td><a href="PostSilverModuleBreadcrumbsGenerator_23560926.html">PostSilverModuleBreadcrumbsGenerator</a></td>
<td>renders breadcrumbs for a silver module element</td>
</tr>
<tr>
<td><a href="EzContentBreadcrumbsGenerator_23560916.html">EzContentBreadcrumbsGenerator</a></td>
<td>renders breadcrumbs for an eZ Platform content element</td>
</tr>
<tr>
<td><a href="CatalogBreadcrumbsGenerator_23560752.html">CatalogBreadcrumbsGenerator</a></td>
<td>renders breadcrumbs for an e-shop catalog element</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [FireShot Capture 171 - Breadcrumb Prototype - Harmony Intern\_ - http\_\_\_confluence.extranet.silvers.png](attachments/23560525/23563402.png) (image/png)  
