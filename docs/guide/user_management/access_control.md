# Access control

## Introduction

eZ Commerce uses the eZ Platform policies to avoid access to some specific pages. The policies can be assigned in the backend to a specific user role. eZ Platform allows you to create your own role system.

## eZ Commerce policies

One policy consists of a module and a function.

|Module|Function|Description|
|--- |--- |--- |
|eZ Commerce|||
|siso_policy|checkout|Can access the checkout process|
|siso_policy|dashboard_view|Can access the backend cockpit|
|siso_policy|forms_profile_edit|Can access the user profile|
|siso_policy|lostorder_list|Can access the lostorders in the backend|
|siso_policy|lostorder_manage||
|siso_policy|lostorder_process||
|siso_policy|quickorder|Can access the quickorder|
|siso_policy|read_basket|Can see the basket|
|siso_policy|write_basket|Can modify the basket (add, update, delete)|
|customercenter|||
|siso_customercenter|approve|Can approve baskets in the customer center|
|siso_customercenter|buy|Can buy as the customer center user|
|siso_customercenter|view|Can access the customer center user management|

## Handling the access control

eZ Commerce has a flexible way, how to handle the access to some specific area.

- Controllers: If there is a defined route, you can simple add the policy in the routing file.
- [eZ Commerce forms](../one_page_forms/one_page_forms.md): you can add the policy in the forms configuration  

!!! note

    When defining the policies, you must follow the **module/function** syntax.

### Defining policies in the routing file

If you want to avoid user to access a page that is rendered by a controller and a route definition exists for it, you can simply do it in the routing file.

``` yaml
siso_quick_order:
    pattern:  /quickorder
    defaults:
        _controller: SisoQuickOrderBundle:QuickOrder:quickOrder
        policy: siso_policy/quickorder
```

For a simple check if user has a definied policy, this configuration is pretty enough. If you need some more complex rules, e.g. check the section or check more policies at once, you need your own implementation in your controller (or other appropriate place).

### How this works in the backround?

There is a central event listener, that will check the configuration from the routing file and user policies on every request. If user does not have a defined policy, an AccessDenied Exception is thrown and forwarded to the [ExceptionListener](../../cookbook/exception_handling.md).

**Silversolutions/Bundle/EshopBundle/EventListener/VerifyUserPoliciesRequestListener.php**

- listens to a "kernel.finish\_request" event

``` php
/**
 * Checks the user policies for given request.
 * Therefore the information for current route from the routing file are evaluated.
 *
 * Example:
 *  silversolutions_basket_show:
 *      pattern:  /basket/show
 *      defaults:
 *          _controller: SilversolutionsEshopBundle:Basket:show
 *          policy: siso_policy/read_basket
 *
 * @param FinishRequestEvent $event
 * @return void
 * @throws AccessDeniedException
 */
public function verifyUserPolicies(FinishRequestEvent $event)
{
    $request = $event->getRequest();
    $routeParams = $request->attributes->get('_route_params');

    if (is_array($routeParams)
        && array_key_exists('policy', $routeParams)
    ) {
        $policy = explode('/', $routeParams['policy']);
        if (is_array($policy)
            && array_key_exists(0, $policy)
            && array_key_exists(1, $policy)
        ) {
            $module = $policy[0];
            $function = $policy[1];

            $isGrantProfileEdit = $this->authorizationChecker->isGranted(
                new AuthorizationAttribute($module, $function)
            );

            if (!$isGrantProfileEdit) {
                throw new AccessDeniedException('access_denied_' . $module . '_' . $function);
            }
        }
    }
}
```
### What happen by missing policy?

When the user tries to call a url, where he needs a special policy, that he doesn't have, **VerifyUserPoliciesRequestListener** is throwing an exception. Then the exception listener is rendering an access denied page.
