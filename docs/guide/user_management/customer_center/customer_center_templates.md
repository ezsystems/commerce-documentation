# Customer center templates

## Template list

Default path: `vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/views/CustomerCenter`

|Path|Description|
|--- |--- |
|index.html.twig`|Entry page for customer center|
|`Basket/messages.html.twig`|Subtemplate that renders an error message if a user exceeds their budget on the basket show page|
|`Basket/remove_basket.html.twig`|Subtemplate that renders the button to delete the current basket on the basket show page|
|`Email/approve_basket.html.twig`|HTML content for the email that is sent to the approver to inform them about a new approval request|
|`Email/approve_basket.html.txt`|Text content for the email that is sent to the approver to inform them about a new approval request|
|`Email/reject_basket.html.twig`|HTML content for the email that is sent to buyer when approver rejected their basket|
|`Email/reject_basket.html.txt`|Text content for the email that is sent to buyer when approver rejected their basket|
|`Email/new_account_pw.html.twig`|HTML content for the email that is sent to a user if the main contact has created a shop account for them|
|`Email/new_account_pw.html.txt`|Text content for the email that is sent to a user if the main contact has created a shop account for them|
|`Approver/send_to_approver.html.twig`|Page that is displayed for the buyer after they went through the approval process|
|`Approver/reject_basket.html.twig`|Page for the approver where they can reject the basket|
|`Approver/buyer.html.twig`|Renders entry page for buyer with approval requests|
|`Approver/approver.html.twig`|Renders entry page for approver with approval requests|
|`Form/add_user.html.twig`|Renders the form for adding a known ERP user to the shop|
|`Form/edit_user.html.twig`|Renders the form for editing user|
|`Form/request_user.html.twig`|Renders the form for requesting new user|
|`Form/base_form.html.twig`|Base template that renders the form data and is used in all `Form/*` templates|
|`vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/views/parts/user_menu.html.twig`|Overrides `user_menu.html.twig`|

## Related custom Twig functions

#### is_customer_center_active

`is_customer_center_active` returns true if Customer center is enabled.

``` html+twig
{% set is_customer_center_active = is_customer_center_active() %}
{% if is_customer_center_active %}
```

#### ses_user_budget

`ses_user_budget` returns user budget as an array.

``` php
 array(
 'budget_order' => 100.00,
 'budget_month' => 400.00
)
```

``` html+twig
{% set userBudget = ses_user_budget(basket.dataMap.buyerUserId) %}
{% if userBudget is iterable %}
    <ul >
        {% for userBudgetKey, userBudgetValue in userBudget %}
            <li>{{ userBudgetKey|st_translate }}: {{ userBudgetValue|price_format(ses.profile.sesUser.customerCurrency) }}</li>
        {% endfor %}
    </ul>
{% endif %}
```

#### is_customer_center_buyer

`is_customer_center_buyer` returns true if the user is a Customer center user and has the buyer Role.

``` html+twig
{% set is_customer_center_buyer = is_customer_center_buyer() %}
{% if is_customer_center_buyer %}
...
```

#### is_customer_center_approver

`is_customer_center_approver` returns true if the user is a customer center user and has the approver Role.

``` html+twig
{% set customer_center_approver = is_customer_center_approver() %}
{% if customer_center_approver %}
...
```

#### is_customer_center_main_contact

`is_customer_center_main_contact` returns true if the user is a Customer center user and has the main contact Role.

``` html+twig
{% if is_customer_center_main_contact() %}
...
```

#### ses_user_order_sum_last_month

`ses_user_order_sum_last_month` returns the sum of user orders in the last month.

``` html+twig
{% set userOrderSum = ses_user_order_sum_last_month(basket.dataMap.buyerUserId) %}
```

### Twig Tests

#### customer_center_contact

`customer_center_contact` returns true if the given contact is already a Customer center contact.

``` html+twig
{% if contact is not customer_center_contact(customer_center_contacts) %}
...
```

#### customer_center_contacts

`customer_center_contacts` returns true if all ERP contacts are already Customer center contacts.

``` html+twig
{% if erp_contacts is not empty
 and erp_contacts is not customer_center_contacts(customer_center_contacts)
%}
...
```
