#  Access control 

# Introduction

eZ Commerce uses the eZ Platform policies to avoid access to some specific pages. The policies can be assigned in the backend to a specific user role. eZ Platform allows you to create your own role system.

## eZ Commerce policies

One policy consists of a module and a function.

<table>
<thead>
<tr class="header">
<th>Module</th>
<th>Function</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>eZ Commerce</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>siso_policy</td>
<td>checkout</td>
<td>Can access the checkout process</td>
</tr>
<tr>
<td>siso_policy</td>
<td>dashboard_view</td>
<td>Can access the backend cockpit</td>
</tr>
<tr>
<td>siso_policy</td>
<td>forms_profile_edit</td>
<td>Can access the user profile</td>
</tr>
<tr>
<td>siso_policy</td>
<td>lostorder_list</td>
<td>Can access the lostorders in the backend</td>
</tr>
<tr>
<td>siso_policy</td>
<td>lostorder_manage</td>
<td>???</td>
</tr>
<tr>
<td>siso_policy</td>
<td>lostorder_process</td>
<td>???</td>
</tr>
<tr>
<td>siso_policy</td>
<td>quickorder</td>
<td>Can access the quickorder</td>
</tr>
<tr>
<td>siso_policy</td>
<td>read_basket</td>
<td>Can see the basket</td>
</tr>
<tr>
<td>siso_policy</td>
<td>write_basket</td>
<td>Can modify the basket (add, update, delete)</td>
</tr>
<tr>
<td>customercenter</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>siso_customercenter</td>
<td>approve</td>
<td>Can approve baskets in the customer center</td>
</tr>
<tr>
<td>siso_customercenter</td>
<td>buy</td>
<td>Can buy as the customer center user</td>
</tr>
<tr>
<td>siso_customercenter</td>
<td>view</td>
<td>Can access the customer center user management</td>
</tr>
</tbody>
</table>

## Handling the access control

eZ Commerce has a flexible way, how to handle the access to some specific area.

  - Controllers: If there is a defined route, you can simple add the policy in the routing file.
  - [eZ Commerce forms](One-page-forms_23560744.html): you can add the policy in the forms configuration  

When defining the policies, you must follow the **module/function** syntax\!

### Defining policies in the routing file

If you want to avoid user to access a page that is rendered by a controller and a route definition exists for it, you can simply do it in the routing file.

``` 
siso_quick_order:
    pattern:  /quickorder
    defaults:
        _controller: SisoQuickOrderBundle:QuickOrder:quickOrder
        policy: siso_policy/quickorder
```

For a simple check if user has a definied policy, this configuration is pretty enough. If you need some more complex rules, e.g. check the section or check more policies at once, you need your own implementation in your controller (or other appropriate place).

### How this works in the backround?

There is a central event listener, that will check the configuration from the routing file and user policies on every request. If user does not have a defined policy, an AccessDenied Exception is thrown and forwarded to the [ExceptionListener](Exception-Handling_23560498.html).

**Silversolutions/Bundle/EshopBundle/EventListener/VerifyUserPoliciesRequestListener.php**

  - listens to a "kernel.finish\_request" event

``` 
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

When the user tries to call a url, where he needs a special policy, that he doesn´t have, **VerifyUserPoliciesRequestListener** is throwing an exception. Then the exception listener is rendering an access denied page.

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-07-31 um 12.51.28.png](attachments/23560922/23563797.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-07-31 um 12.50.48.png](attachments/23560922/23563987.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-07-31 um 12.50.48.png](attachments/23560922/23563798.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-07-31 um 12.58.42.png](attachments/23560922/23563988.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-10-22 um 09.59.28.png](attachments/23560922/23563473.png) (image/png)  
