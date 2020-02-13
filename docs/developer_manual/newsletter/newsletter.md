#  Newsletter 

## Introduction

eZ Commerce offers a newsletter interface. This allows user to un/subscribe from/to newsletters and see the newsletter status or update newsletter details information in his profile.

In standard implementation of %ses-band there is no specific newsletter provider connection. The standard offers only processes, templates, routes, configurations and interface which can be used for newsletter integration. So only a newsletter provider service with specific API implementation of this provider has to be implemented. One plugin for eZ Commerce is Newsletter2Go.

### Newsletter features in eZ Commerce

  - Double-opt-in process (via shop) when user subscribes to newsletter

  - User can subscribe from the newsletter box

  - User can subscribe during the registration process

  - User can un/subscribe from his profile

  - User can update his newsletter details from his profile

  - User newsletter details are updated after ordering in the checkout

  - The newsletter status is fetched from the newsletter provider after the login and user see the status in his profile. The newsletter status is stored in the CustomerProfileData together with the list of ids of newsletter topics, so it can be rendered in the template if required.
    ``` 
    {# true if user subscribed at least one newsletter #}
    {{ ses.profile.sesUser.subscribesNewsletter }}
    
    {# list of subscribed newsletter ids #}
    {% set dataMap = ses.profile.getDataMap.dataMap %}
    {% set newsletters = dataMap.subscribed_list_ids %}
    {% if newsletters|default is not empty %}
      {% for newsletter in newsletters %}
        {{ newsletter }}
      {% endfor %}
    {% endif %}
    ```

#### Default attributes

Following attributes are offered by default. These attributes ('created on' is an exception this is set by newsletter2go automatically) are send to the newsletter provider if available:

  - First name
  - Last name
  - Gender
  - Telephone Number
  - Date of birth
  - User language
  - Last order date
  - last order amount
  - Order amount total

#### Additional attributes

There are places where you can add or modify additional user data and send it to newsletter provider.

1.  Before a user subscribes to the newsletter.  In standard the user language (current locale) is sent.  
    Any data from the [customer profile](Customers_23560704.html) can be sent, however custom EventListener has to be implemented for that, see [FAQ](/pages/createpage.action?spaceKey=EZC14&title=Newsletter2Go+-+FAQ&linkCreation=true&fromPageId=23560215).
2.  After user updates his profile or creates an order. In standard the newsletter details will be updated with latest information about:  
      - last\_order\_date - date when last order was made 
      - last\_order\_amount - order amount from last order
      - order\_amount\_total - total amount for the user
    Any data from the [customer profile](Customers_23560704.html) can be sent, however custom EventListener has to be implemented for that, see [FAQ](/pages/createpage.action?spaceKey=EZC14&title=Newsletter2Go+-+FAQ&linkCreation=true&fromPageId=23560215).  

Attributes, that do not exist in the newsletter provider has to be created first.

## Double-opt-in (DOI) process

When user subscribes to newsletter via shop (newsletter box, registration, profile), shop first checks if user already exists in the newsletter provider. It can happen, that user already subscribed in the past.

If the user is not known to the newsletter provider, DOI process is started.

User data is stored in the shop in a [token](Token_23560932.html), and the user receives an email, where he has to confirm his email address.

After user clicked on the link in the confirmation email, the token is invalidated and user is subscribed to the newsletter. If he is logged in, he can immediately update the status in his profile.

## Important terms used in this document

ERP

**ERP** (Enterprise resource planning) is business [management](http://en.wikipedia.org/wiki/Management "Management") software—typically a suite of integrated applications—that a company can use to collect, store, manage and interpret data from many business activities, including:

  - [Product planning](http://en.wikipedia.org/wiki/Product_planning "Product planning"), cost
  - [Manufacturing](http://en.wikipedia.org/wiki/Manufacturing "Manufacturing") or service delivery
  - [Marketing](http://en.wikipedia.org/wiki/Marketing "Marketing") and sales
  - [Inventory management](http://en.wikipedia.org/wiki/Inventory_management "Inventory management")
  - Shipping and payment

*Source*: Wikipedia

Known ERP software packages:

  - <span class="internal">Microsoft Dynamics NAV (former navision)
  - <span class="internal">Microsoft Dynamics AX  
    
  - SAP

## Before you start 

Please keep in mind that newsletter is really connected with a lot of different modules with the webshop. Be sure to check these out:

  - [CustomerProfileData](Customers_23560704.html)
  - [Landing page tool](/pages/createpage.action?spaceKey=EZC14&title=Landing+page+tool&linkCreation=true&fromPageId=23560215)

## FAQ

![](http://ezcommerce-demo.ezplatform.com/bundles/silversolutionseshop/img/logo.png)

## Templating

See [Newsletter - Templates](Newsletter--Templates_23560212.html)

## Cookbook

Check our recipies to learn more about the eZ Commerce newsletter implementation:

  - [How to send additional data to the Newsletter provider](https://doc.silver-eshop.de/display/EZC14/How+to+send+additional+data+to+the+Newsletter+provider)

## API

See following documentation to find out, how to work with the newsletter API:

  - [Newsletter Configuration](https://doc.silver-eshop.de/display/EZC14/Newsletter+Configuration)
  - [Newsletter Interface](https://doc.silver-eshop.de/display/EZC14/Newsletter+Interface)

## Attachments:

![](images/icons/bullet_blue.gif) [FireShot Capture 192 - Active Contacts - https\_\_\_ui.newsletter2go.com\_recipients\_oty804fn.png](attachments/23560215/23570793.png) (image/png)  
![](images/icons/bullet_blue.gif) [FireShot Capture 193 - Traits - https\_\_\_ui.newsletter2go.com\_recipients\_oty804fn\_data-fields.png](attachments/23560215/23570794.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2016-11-07 um 11.05.29.png](attachments/23560215/23570795.png) (image/png)  
