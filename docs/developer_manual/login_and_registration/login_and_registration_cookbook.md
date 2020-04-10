# Login and registration cookbook

## Login process adaptation in 3 steps

The goal of this document is to describe login functionally, which is extending symfony default login process.

eZ Commerce is able to check credentials not only specified by username and password, but also using customer number (which could be taken from ERP system). 

## Services

### AuthenticationListener

The very first part in the authentication process is the AuthenticationListener. This listener is able to read the posted values from the login form and create a UserToken.

The default Symfony listener had to be extended to collect more information (`customer_number`). In the standard Symfony login form only username and password are allowed.

Our extended listener implements the same interface AbstractAuthenticationListener.

`services.xml`:

``` xml
<parameter key="security.authentication.listener.form.class">Silversolutions\Bundle\EshopBundle\Service\Security\CustomFormAuthenticationListener</parameter>
```

We have to create different UsernamePasswordToken (using TokenInterface) here. We have to add new attribute to the token "customer\_no" . This UsernamePasswordToken will be passed to next service in the authentication process.

``` php
$token = new UsernamePasswordToken($username, $password, $this->providerKey);
$token->setAttribute('customer_no', $customerNo);
```

### UserProvider

UserProvider gets the created UsernamePasswordToken and his goal is to fetch the correct user from eZ.

The default fetching functionality had to be extended because in the customer center user can be placed in different user groups (or without a group).

It uses "**locationId**" in the ez backend to determine **private/b2b customers.**

It provides a checkEzUser method which checks the location and customer number of the given user.

``` xml
<parameter key="ezpublish.security.user_provider.class">Silversolutions\Bundle\EshopBundle\Service\Security\UserProvider</parameter>
```

### AuthenticationProvider

The very first part in the authentication process is the AuthenticationListener. This listener is able to read the posted values from the login form and create a UserToken.

The default Symfony listener had to be extended to collect more information (`customer_number`). In the standard Symfony login form only username and password are allowed.

Our extended listener implements the same interface AbstractAuthenticationListener. 

`services.xml`:

``` xml
<parameter key="security.authentication.provider.dao.class">Silversolutions\Bundle\EshopBundle\Service\Security\AuthenticationProvider</parameter>
```

## Security Controller

The goal of the Security Controller - login action - is to display the login mask and authentication errors, if there are some.

``` php
 /**
     * renders the login form
     *
     * @param \Symfony\Component\HttpFoundation\Request $request
     * @return Response
     */
    public function loginAction(Request $request)
    {
        $session = $request->getSession();
        if ($request->attributes->has(SecurityContextInterface::AUTHENTICATION_ERROR)) {
            $error = $request->attributes->get(SecurityContextInterface::AUTHENTICATION_ERROR);
        } else  {
            $error = $session->get(SecurityContextInterface::AUTHENTICATION_ERROR);
            $session->remove(SecurityContextInterface::AUTHENTICATION_ERROR);
        }
        return $this->render(
            'SilversolutionsEshopBundle:Security:login.html.twig',
            array(
                'error' => $error,
            )
        );
    }
```

## Login Form

Login form was adjusted to use additional parameter `_customer_no`.

#### Template Example

`views/Security/login.html.twig`:

```html+twig
<form class="" action="{{ path( 'login_check' ) }}" method="post" role="form">
     {% block login_fields %}
           <fieldset>
                 {% if ses_config_parameter('enable_customer_number_login','siso_eshop') %}
                    <label for="customer_no">{{ 'Customer Number'|st_translate }}</label>
                    <input type="text" id="customer_no" class="form-control" name="_customer_no" placeholder="{{ 'Customer Number'|st_translate }}" />
                 {% endif %}
                 <div class="form-group">
                            <label for="username">{{ 'Username / Email'|st_translate }}</label>
                            <input type="text" id="username" class="form-control" name="_username" value="" required="required" autofocus="autofocus" autocomplete="on" placeholder="{{ 'Username'|st_translate }}" />
                 
                 <div class="form-group{% if error %} has-error{% endif %}">
                            <label for="password">{{ 'Password'|st_translate }}</label>
                            <input type="password" id="password" class="form-control" name="_password" required="required" placeholder="{{ 'Password'|st_translate }}" />
                 
                 <button type="submit" class="button">{{ 'Login'|trans }}</button>
           </fieldset>
     {% endblock %}
  </form>
```

!!! note

    The login forms in the template must post to the `{{ path( 'login_check' ) }}`

    Please pay attention that this URL is NOT the url of the security controller. The posted data is handled automatically by Symfony and pasted to the Security context, where the authentication is done.

## How to log in User from a controller?

Sometimes it is necessary that the User is logged in directly, without typing of username/password combination for some reasons, for example you are using login via SSO and want to login the user in your application.

``` php
public function loginEzUserAction()
{
        $roles = array('ROLE_USER');
        $repository = $this->get('ezpublish.api.repository');
        $ezUser = $this->repository->getCurrentUser();
        $user = new CoreUser($ezUser, $roles);

         /**
             * the provider key can be found usually in the security.yml file
             * it is the name of the firewall
             * e.g.
             * firewalls:
             *      ezpublish_front:
             *          pattern: ^/
             *          anonymous: ~
             *          form_login:
             *          require_previous_session: false
             *          logout: ~
        */
        $providerKey = 'ezpublish_front';

        //create new token
        $token = new UsernamePasswordToken($user, null, $providerKey, $roles);
        $this->securityContext->setToken($token);
        $this->repository->setCurrentUser($ezUser);
}
```
