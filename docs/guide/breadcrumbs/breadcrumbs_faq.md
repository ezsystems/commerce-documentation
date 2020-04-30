# Breadcrumbs FAQ

## Why do the breadcrumbs for my controller only display the root element?

Please check the routing.yml.

The `RoutesBreadcrumbsGenerator` need at least the parameter `breadcrumb_path`. This parameter usually contains the key of the current routing definition.

``` yaml
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: custom_blog_index
        breadcrumb_names: Blog List
```

## Some or all elements of the breadcrumbs display strange words like `some_route_name|breadcrumb`?

This can be caused by two issues in `routing.yml`.

1\. No `breadcrumb_names` is defined:

``` yaml
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: custom_blog_index
```

2\. The number of elements in `breadcrumb_names` and `breadcrumb_path` differ:

``` yaml
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: custom_blog_index/new
        breadcrumb_names: Blog List
```

In these cases, the fallback implementation tries to translate the respective `breadcrumb_path` elements with context `breadcrumb`, which most likely is not translated. The best solution is to have proper path and names configuration and existing translations for the elements of `breadcrumb_names`:

``` yaml
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: custom_blog_index/new
        breadcrumb_names: Blog List/New blog post
```

## Why are no breadcrumbs displayed at all?

Possibilities:

- The `breadcrumb` block of the `pagelayout.html.twig` template was overridden by the currently displayed, extending template with empty content.
- The matched generator encountered an error and didn't render the breadcrumbs
- Very unlikely but not impossible: No generator matched at all.
In the standard setup, the lowest priority `RoutesBreadcrumbsGenerator` checks the active Router service to match the active Request service. That should always be the case.

## How can breadcrumbs be limited to subtrees in sub-shops?

The parameter `content.tree_root.location_id` is used to limit the sub-shops to their subtrees (contains Location ID of the desired catalog).

If `content.tree_root.location_id` is set, a Criterion is used in the `CatalogHelper.php` to fetch the correct product catalog instead of the default one.

``` php
if ($this->configResolver->hasParameter('content.tree_root.location_id')) {
                $ezLocationRootId = $this->configResolver->getParameter('content.tree_root.location_id');
                $ezLocationRoot = $this->locationService->loadLocation($ezLocationRootId);
                $ezLocationRootPath = $ezLocationRoot->pathString;
                $subtreeCriterion = new Criterion\Subtree($ezLocationRootPath);
                $criteria[] = $subtreeCriterion;
            }
```

## What is the purpose of the additional data stored in `translationParameters`?

This data can be used for example to define if a breadcrumb of a Location should be clickable or hidden.

The following example makes breadcrumbs bold and not clickable (if `crumb.translationParameters.type == 'catalog'`):

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Breadcrumbs/breadcrumb_list.html.twig`:

``` html+twig
{% if not loop.last %}
  <li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
    {% if crumb.translationParameters.type == 'catalog' %}
      <b>
        <span itemprop="name">{{ crumb.text }}
        <meta itemprop="position" content="{{ loop.index }}"/>
      </b>
    {% else %}
      <a itemprop="item" href="{{ crumb.url }}">
        <span itemprop="name">{{ crumb.text }}
      </a>
      <meta itemprop="position" content="{{ loop.index }}" />
    {% endif %}
  </li>
{% else %}
  {# TODO make last element link configurable #}
  <li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
    <span itemprop="name">{{ crumb.text }}
    <meta itemprop="position" content="{{ loop.index }}" />
  </li>
{% endif %}
```

![](../img/breadcrumbs_faq.png)  
