#  Wishlist & Stored Baskets 

## Introduction

eZ Commerce offers feature to save your favorite products in a list, so the customer can easily access them or add them to his cart. There are 2 options for storing: 

  - **Wishlist** 
  - custom named lists called **Stored Basket**.

This feature is available **only** **for** the customers that have an account in eZ Commerce and are **logged in**.

The difference between Wishlist and Stored Basket is that there is only **one** **wishlist** per user, but there can be **many stored baskets** with different names for one user. 

Comparison table: 

| Feature                | Wishlist                                     | Stored basket |
| ---------------------- | -------------------------------------------- | ----------------------------------------------------- |
| More than one list     | ![(error)](images/icons/emoticons/error.png) | ![(tick)](images/icons/emoticons/check.png)           |
| Add products to basket | ![(tick)](images/icons/emoticons/check.png)  | ![(tick)](images/icons/emoticons/check.png)           |
| Store the quantity     | ![(error)](images/icons/emoticons/error.png) | ![(tick)](images/icons/emoticons/check.png)           |
| Give list a name       | ![(error)](images/icons/emoticons/error.png) | ![(tick)](images/icons/emoticons/check.png)           |
| Show prices            | ![(error)](images/icons/emoticons/error.png) | ![(tick)](images/icons/emoticons/check.png)           |

The wishlist and stored basket function is based on the basket system of eZ Commerce. 

## Before you start 

Please keep in mind that Wishlist/Stored Baskets is really connected with different modules in our shop. Be sure to check these out:

  - [Basket](http://confluence.ng.silverproducts.de/display/EX/Basket)
  - [BasketService](BasketService_23560232.html)

The stored basket feature can be deactivated in a config file:

``` 
twig:
    globals:
        
        stored_baskets_active: false
```

## Wishlist Features

### Who is able to use wishlist

Wishlist is available only for logged users \!\! There is 1 wishlist per 1 user \!\!

There is no possibility to have more than one wishlist. If a client wants to store some products in different list, he can use stored basket functionality.

### How to add product to wishlist

Add to wishlist link is placed in product detail page in the right column. See below:

![](attachments/23560660/23563865.png)

For products that are **variants** user need to choose actual variant to be able to store it. Add to wishlist will be not visible, until options are selected. See below: 

![](attachments/23560660/23563862.png)

### Messages that are shown when adding to wishlist

After successful adding to wishlist a message is displayed. 

![](attachments/23560660/23563867.png)

Items can be stored only once. Notice is displayed:

![](attachments/23560660/23563864.png)

When adding product:

  - **no** **prices** are stored
  - **no quantity** is stored

### Where to find wishlist links

Wishlist can be found in the header of the website and in the profile page in right user menu. See below (Mein Merkzettel):

**Header**

![](attachments/23560660/23563789.png)

**Profile**

![](attachments/23560660/23563877.png)

### Wishlist overview page

In the overview there are some information about product:

  - name
  - sku
  - short description
  - image
  - variant information

![](attachments/23560660/23563868.png)

#### Adding products to cart

There are 2 ways to add items to cart:

  - add 1 product into cart 
  - add all products into cart

**Quantity field**

When adding into basket the user can define how many items of product will he want to add to cart. If there is no quantity field the minimum order quantity will be taken.

Products will stay in the wishlist as long as user does not remove them.

#### Remove products from wishlist

User can remove products from wishlist one by one. They will not be visible anymore in wishlist.

#### Product not available

If the product is not in catalog anymore, user will see a proper message in the overview page.

## Stored Basket Features

### Who is able to use stored basket

Stored basket is available only for logged users \!\! There can be many stored baskets per 1 user \!\!

Every stored basket have a name, that has to be provided by user. 

User can store the whole basket under a name for later purposes and new stored basket will be created.

### How to add products to stored basket 

There are 2 ways to add products to stored basket:

  - User can **add** an item into a *stored basket* from product detail page. See below:  
    ![](attachments/23560260/23563902.png)

<!-- end list -->

  - User can add all items from the basket into stored basket (user must choose stored basket from a list or provide new one).  
    ![](attachments/23560260/23563888.png)

There are 2 options user can take within the popup:

  - **save as new stored basket** - if the user doesn't have any *stored baskets* yet, a pop-up-window is shown, with similar text: '*You don't have any stored baskets yet. Please enter a name for your stored basket*'. Here an input field can be displayed where the user must enter the basket name.
  - **choose existing stored basket** - if the user has some *stored baskets* already, a list (pop-up-window) is shown, where the user can choose which *stored basket* he wants to add item into. Also he has possibility to enter a new basket name.    

If the product is a **variant** user needs to choose all options to be able to add product.

For stored baskets the price and quantity for product is stored \!\!\!

### Where to find stored basket links

User can find the list of *stored baskets* in his profile page. He can click on one of the stored baskets and see the details. 

#### List of stored baskets

User can find a list of all his stored baskets in the shop functions.

On the list page he has an overview, can choose or delete one of the stored baskets.

![](attachments/23560260/23563733.png)![](attachments/23560260/23563732.png)  

#### Stored basket detail page

The prices in the stored basket are updated immediatelly after user entered the stored basket page.

In the overview there are some information about product:

  - name
  - sku
  - short description
  - image
  - variant information
  - price and availability
  - the stored quantity - possible to change for add to basket functionality

![](attachments/23560260/23563876.png)

#### Adding products to basket

There are 2 ways to add items to basket:

  - add 1 product into basket 
  - add all products into basket

**Quantity field**

When adding into basket the user can define how many items of product will he want to add to basket. If there is no quantity field the minimum order quantity will be taken.

Products will stay in the stored basket as long as user does not remove them.

#### Remove products from stored basket

User can remove products in 2 ways:

  - delete an item from *stored basket*
  - delete the whole stored basket (trash icon in the right user menu section)

#### Product not available

If the product is not in catalog anymore, user will see a proper message in the overview page.

## Templates

### Templates list:

Default path:

    vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Path
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>Basket/stored_baskets_list.html.twig</code></pre></td>
<td>list of all stored baskets</td>
</tr>
<tr>
<td><pre><code>Basket/show_stored_basket.html.twig</code></pre></td>
<td>entry page for wishlist and stored baskets. Based on the basket type it loads one of the templates listed below</td>
</tr>
<tr>
<td><pre><code>Basket/show_stored_basket_part.html.twig</code></pre></td>
<td>partial page responsible for rendering stored basket page</td>
</tr>
<tr>
<td><pre><code>Basket/show_wishlist_part.html.twig</code></pre></td>
<td>partial page responsible for rendering wishlist page</td>
</tr>
<tr>
<td><pre><code>Basket/messages.html.twig</code></pre></td>
<td>template with success/error/notice messages for baskets</td>
</tr>
</tbody>
</table>

### Related custom Twig modifiers/functions/etc/:

#### Twig functions

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Twig function
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
<th><div class="tablesorter-header-inner">
Usage
</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>get_stored_baskets</code></pre></td>
<td><pre><code>returns stored baskets for current user</code></pre></td>
<td><pre><code>{% set storedBaskets = get_stored_baskets() %}</code></pre>
<pre><code>{% if storedBaskets|default is not empty %}</code></pre>
<pre><code>...</code></pre></td>
</tr>
</tbody>
</table>
