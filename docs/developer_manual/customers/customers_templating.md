#  Customers - Templating 

The eZ Commerce provides a global Twig template variable "`ses`" which is available in all Twig templates. The variable provides a method "`profile`" which will provide the information about the current customer which is using the shop.

If the user has a customer number, eZ Commerce will automatically fetch information from the ERP sending a customer request, the data will be stored in the session and will be provided by the variable "`ses.profile`". Subsequent calls will not initiate a new request to the ERP since the data from the ERP will be cached and handled by the Symfony session handlers.

eZ Commerce provides a standard template for displaying customer data:

|          |                                                                |
| -------- | -------------------------------------------------------------- |
| Location | `SilversolutionsEshopBundle/Resources/views/details.html.twig` |

# Getting customer profile data

As mentioned in [Twig extension](http://confluence.ng.silverproducts.de/display/EX/Twig+extension) there is a sub-object "`profile`" within the global "`ses`" template variable. As "`ses.profile`" in template returns the currently logged in profile, you are able to use all read-only members from the [`CustomerProfileData`](Customer-profile-data-model_23560898.html) implementation. A few examples:

``` 
Current customer number: {{ ses.profile.sesUser.customerNumber }}
All delivery addresses:  {% set deliveryAddresses = ses.deliveryParty %}
E-Mail address:          {{ ses.profile.sesUser.email }}
 
{# check, if user is logged in and blocked: #}
{% if ses.profile.sesUser.isLoggedIn %}
    Hello customer #{{ ses.profile.sesUser.customerNumber }}.
 
    {% if ses.profile.sesUser.contact.isBlocked %}
        Sorry, but you are blocked!
    {% endif %}
{% endif %}
 
{# check, if user is logged in as anonymous user: #}
{% if ses.profile.sesUser.isAnonymous %}
    <p>Anonymous user</p>
{% endif %}
```

As you see in the examples above, you can use **any** data from `CustomerProfileData` instance, see [example in the model](Customer-profile-data-model_23560898.html).

## Getting data from a buyer party

``` 
{#  Example for getting customized data from the ERP stored in the BuyerParty   #}
 
{% set buyerParty = ses.defaultBuyerAddress %}
{% if buyerParty.SesExtension.value.Gliederungskennzeichen is defined and buyerParty.SesExtension.value.Gliederungskennzeichen == '1' %}
 
{% endif %}
 
{#  Get the invoiceParty   #}
{% set invoiceAddress = ses.defaultInvoiceParty %}
 
{# Get the delivery address #} 
{% set deliveryAddress = ses.defaultDeliveryParty %}
       
```

To output the telephone number of the contact, you have to point to the member variable `$phoneNumber` of the `Contact`.

``` 
Phone number of contact {{ ses.profile.sesUser.contact.phoneNumber }}
```

![](attachments/23560911/23563985.png)

## Attachments:

![](images/icons/bullet_blue.gif) [CustomerProfileData\_example\_template.png](attachments/23560911/23563985.png) (image/png)  
