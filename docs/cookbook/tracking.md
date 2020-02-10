#  Tracking 

Silver.e-shop comes with a simple extendible system for tracking tools. Any tracking tool can be plugged in - in a simple way.

The tracking tools are able to output the tracking code in the template, where they are included (product detail, basket...) on a desired position on the page (top/bottom). The tracking tools are included directly in the template by usage of [twig helper methods](#Tracking-twig_helpers).

The tracking services will be executed one after each other depending on the priority.

Every tracking tool service must implement the *TrackerInterface* and must be tagged as *'tracker'* and have a defined priority.

## TrackerInterface

The purpose of this interface is to return rendered tracking code snippets and a desired position, where these snippets should be placed (top/bottom).

**Silversolutions\\Bundle\\EshopBundle\\Api\\TrackerInterface**

``` 
public function getBasketTrackingHtmlResult(Basket $basket, $mode, $params = array());

public function getProductTrackingHtmlResult(ProductNode $catalogElement, $mode, $params = array());

public function getBaseTrackingHtmlResult($params = array()); 
```

## Twig

There are two Twig macros, which implement the common loop, that prints all tracker contents for a specific position. The macros are implemented in:

`Silversolutions/Bundle/EshopBundle/Resources/views/Components/tracker_macros.html.twig`

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Macro name</th>
<th>Parameters</th>
<th>Definition</th>
</tr>
</thead>
<tbody>
<tr>
<td>top</td>
<td><pre><code>tracking_codes (array)</code></pre></td>
<td>

<pre class="" data-syntaxhighlighter-params="brush: text; gutter: false; theme: Confluence" data-theme="Confluence"><code>{% macro top(tracking_codes) %}
    {% for tracking_html in tracking_codes[constant(&#39;Silversolutions\\Bundle\\EshopBundle\\Api\\TrackerInterface::POSITION_TOP&#39;)] %}
        {{ tracking_html|raw }}
    {% endfor %}
{% endmacro %}</code></pre>

</td>
</tr>
<tr>
<td>bottom</td>
<td><pre><code>tracking_codes (array)</code></pre></td>
<td>

<pre class="" data-syntaxhighlighter-params="brush: text; gutter: false; theme: Confluence" data-theme="Confluence"><code>{% macro bottom(tracking_codes) %}
    {% for tracking_html in tracking_codes[constant(&#39;Silversolutions\\Bundle\\EshopBundle\\Api\\TrackerInterface::POSITION_BOTTOM&#39;)] %}
        {{ tracking_html|raw }}
    {% endfor %}
{% endmacro %}</code></pre>

</td>
</tr>
</tbody>
</table>

In order to get the tracking code that is supposed to be passed to the macros, there are several Twig template functions.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Parameters</th>
<th>Purpose</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>ses_track_basket</code></pre></td>
<td><pre><code>Basket $basket
$mode (view, add, buy...)
$params = array()</code></pre></td>
<td><p>This method will execute all tagged services and return a list of rendered basket</p>
tracking contents depending on given parameters.</td>
<td>

<strong>Silversolutions/Bundle/EshopBundle/Resources/views/Basket/show.html.twig</strong>
<pre class="" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: DJango" data-theme="DJango"><code>{% import &quot;SilversolutionsEshopBundle:Components:tracker_macros.html.twig&quot;|st_resolve_template as tracker %}
{% set tracking_codes = ses_track_basket(basket, &#39;view&#39;) %}
{% block tracker_top %}
    {{ tracker.top(tracking_codes) }}
{% endblock %}
...
{% block tracker_bottom %}
    {{ tracker.bottom(tracking_codes) }}
{% endblock %}</code></pre>

</td>
</tr>
<tr>
<td><pre><code>ses_track_product</code></pre></td>
<td><pre><code>ProductNode $catalogElement
$mode (view, ad, buy...)
$params = array()</code></pre></td>
<td><p>This method will execute all tagged services and return a list of rendered product</p>
tracking contents depending on given parameters.</td>
<td>

<strong>Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/product.html.twig</strong>
<pre class="" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: DJango" data-theme="DJango"><code>{% import &quot;SilversolutionsEshopBundle:Components:tracker_macros.html.twig&quot;|st_resolve_template as tracker %}
{% set tracking_codes = ses_track_product(catalogElement, &#39;view&#39;) %}
{% block tracker_top %}
    {{ tracker.top(tracking_codes) }}
{% endblock %}
...
{% block tracker_bottom %}
    {{ tracker.bottom(tracking_codes) }}
{% endblock %}</code></pre>

</td>
</tr>
<tr>
<td><pre><code>ses_track_base</code></pre></td>
<td><pre><code>$params = array()</code></pre></td>
<td><p>This method will execute all tagged services and return a list of rendered base</p>
tracking contents depending on given parameters.</td>
<td>

<strong>Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig</strong>
<pre class="" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: DJango" data-theme="DJango"><code>{% import &quot;SilversolutionsEshopBundle:Components:tracker_macros.html.twig&quot;|st_resolve_template as tracker %}
{% set tracking_codes_base = ses_track_base() %}
{% block tracker_top_base %}
    {{ tracker.top(tracking_codes_base) }}
{% endblock %}
...
{% block tracker_bottom_base %}
    {{ tracker.bottom(tracking_codes_base) }}
{% endblock %} </code></pre>

</td>
</tr>
</tbody>
</table>

## Example: RecommendationTracker

As an example, you can try to implement a recommendation tracker service. Your service must implement the [TrackerInterface](#Tracking-tracker_interface). You can get any configuration or other services, that you may need. The implementation is up to you.

``` 
namespace Company\ProjectBundle\Service;

use Silversolutions\Bundle\EshopBundle\Api\TrackerInterface;

class RecommendationTrackerService implements TrackerInterface
{

    /**
    * get the dependencies
    */
    public function __construct(...)
    {
        ...
    }

    /**
    * returns an array with rendered template snippet and the required position
    * depending on given parameters in following format
    *  array(
    *      'position' => self::POSITION_TOP
    *      'template' => '<script...'
    * )
    *   
    * @param Basket $basket
    * @param string $mode
    * @param array $params
    * @return array
    *
    */
    public function getBasketTrackingHtmlResult(Basket $basket, $mode, $params = array())
    {
        // TODO: Implement getBasketTrackingHtmlResult() method.
    }

    /**
    * returns an array with rendered template snippet and the required position
    * depending on given parameters in following format
    *  array(
    *      'position' => self::POSITION_TOP
    *      'template' => '<script...'
    * )
    *
    * @param ProductNode $catalogElement
    * @param string $mode
    * @param array $params
    * @return array
    *
    */
    public function getProductTrackingHtmlResult(ProductNode $catalogElement, $mode, $params = array())
    {
        // TODO: Implement getProductTrackingHtmlResult() method.
    }

    /**
    * returns an array with rendered template snippet and the required position
    * depending on given parameters in following format
    *  array(
    *      'position' => self::POSITION_TOP
    *      'template' => '<script...'
    * )
    *
    * @param array $params
    * @return array
    *
    */
    public function getBaseTrackingHtmlResult($params = array())
    {
        // TODO: Implement getBaseTrackingHtmlResult() method.
    }
}
```

You must define the implementation as a symfony service and tag it as *"tracker"* with a mandatory *priority* attribute. With the priority you can customize the order of invocation for all tracking services (lower number = earlier invocation).

``` 
    <parameters>
        <parameter key="company_tracking.recommendation_tracker.class">Company\ProjectBundle\Service\RecommendationTrackerService</parameter>
    </parameters>

    <services>
        <service id="company_tracking.recommendation_tracker" class="%company_tracking.recommendation_tracker.class%">
            <argument type="service" id="templating" />
            <argument type="service" id="siso_tools.template_resolver" />
            <argument>%recommendation_customer_id%</argument>            
            <tag name="tracker" priority="1" />
        </service>
    </services>
```
