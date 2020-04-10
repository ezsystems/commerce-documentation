# RoutesBreadcrumbsGenerator

This generates the breadcrumbs for the symfony routes.

## FQN

Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\RoutesBreadcrumbsGenerator

## Criterion for responsibility

If the eZ\Publish\Core\MVC\Symfony\Routing\ChainRouter matches the request, this generator is responsible. Since this is the case for nearly every request, this generator must be registered with (one of) the lowest priority(/ies).

## Notes to the rendering process

Renders breadcrumbs, if the route for the currently matched controller action contains additional information for its the breadcrumbs (routing.yml), for example:

``` yaml
siso_global_search:
    path: /search_query
    defaults:
        _controller: SisoSearchBundle:Search:search
        breadcrumb_path: siso_global_search
        breadcrumb_names: Search
```

- `breadcrumb_path` defines a character ('/') separated sequence of route identifiers, which represents the links of the breadcrumbs elements.
- `breadcrumb_names` is a similar sequence of same order and number as `breadcrumb_path` and specifies the displayed names of the breadcrumb elements. The values are not rendered directly, but define the source for a translation.

!!! note "Limitation"

    There is one limitation when using the breadcrumb for the custom routes. When you want to restrict the HTTP method for the controller, you can not do it in the routing file.

    ``` yaml
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

    ``` yaml
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
