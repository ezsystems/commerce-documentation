# Customer Center - Templates 

### Templates list:

Default path: vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/views/CustomerCenter

|Path|Description|
|--- |--- |
|index.html.twig|entry page for customer center|
|Basket/messages.html.twig|subtemplate that will render an error message, if user exceeds his budget on the basket show page|
|Basket/remove_basket.html.twig|subtemplate that will render the button to delete the current basket on the basket show page|
|Email/approve_basket.html.twig|html content for the email that is send to the approver to inform him about new approval request|
|Email/approve_basket.html.txt|text content for the email that is send to the approver to inform him about new approval request|
|Email/reject_basket.html.twig|html content for the email that is send to buyer when approver rejected his basket|
|Email/reject_basket.html.txt|text content for the email that is send to buyer when approver rejected his basket|
|Email/new_account_pw.html.twig|html content for the email that is send to user if main contact has created a shop account for him|
|Email/new_account_pw.html.txt|text content for the email that is send to user if main contact has created a shop account for him|
|Approver/send_to_approver.html.twig|page that is displayed for buyer after he went through the approval process|
|Approver/reject_basket.html.twig|page for the approver where he can reject the basket|
|Approver/buyer.html.twig|renders entry page for buyer with the approval requests|
|Approver/approver.html.twig|renders entry page for approver with the approval requests|
|Form/add_user.html.twig|renders the form for add known erp user to the shop|
|Form/edit_user.html.twig|renders the form for edit user|
|Form/request_user.html.twig|renders the form for request new user|
|Form/base_form.html.twig|base template that is renders the form data and is used in all Form/* templates|
|vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/views/parts/user_menu.html.twig|overrides user_menu.html.twig from silver-e.shop|


### Related custom Twig modifiers/functions/etc/:

### Twig functions

#### is_customer_center_active

returns true if customer center is enabled

```
{% set is_customer_center_active = is_customer_center_active() %}
{% if is_customer_center_active %}
...
```

#### ses_user_budget

returns user budget as an array 
Example:

```
 array(
 'budget_order' => 100.00,
 'budget_month' => 400.00
)
```

```
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

returns true if user is a customer center user and has the buyer role

```
{% set is_customer_center_buyer = is_customer_center_buyer() %}
{% if is_customer_center_buyer %}
...
```

#### is_customer_center_approver

returns true if user is a customer center user and has the buyer role

```
{% set customer_center_approver = is_customer_center_approver() %}
{% if customer_center_approver %}
...
```

#### is_customer_center_main_contact

returns true if user is a customer center user and has the buyer role

```
{% if is_customer_center_main_contact() %}
...
```

#### ses_user_order_sum_last_month

returns the sum of user orders of the last month

```
{% set userOrderSum = ses_user_order_sum_last_month(basket.dataMap.buyerUserId) %}
```

### Twig Tests

#### customer_center_contact

returns true if given contact is already customer center contact

```{% if contact is not customer_center_contact(customer_center_contacts) %}
...
```

#### customer_center_contacts

returns true if all erp contacts are already customer center contacts

```
{% if erp_contacts is not empty
 and erp_contacts is not customer_center_contacts(customer_center_contacts)
%}
...
```
