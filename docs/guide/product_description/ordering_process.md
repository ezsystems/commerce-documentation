# Ordering process

## Ordering

### Quickorder

This form supports the ordering process for B2B customers and reduces the time to set up an order.

A customer can enter several products and add them to the basket.

![](img/image2018-4-4_18-53-18.png)

An upload function allows to upload a csv file containing a list of skus and quantities. Even more convenient is the drag & drop function.

Variant products can be added as well.

![](img/image2018-4-4_18-55-39.png)

### Add to basket

The user can add products (not a variant) product to the basket directly from the search result in the auto suggest function,

![](img/autosuggest.png)

from the product catalog, the product detail page,

![](img/product_catalog.png)

![](img/product-detail.png)

from the wishlist and comparison list

![](img/wishlist.png)

![](img/comparison_list.png)

from stored basket

![](img/stored_basket_2.png)

and from order history.

![](img/order_history.png)

## The checkout

### The basket and stored basket

The basket allows to change quantities and to set a remark per order line if this was enabled in the configuration settings. 

![](img/basket.png)

The basket can be stored as a new stored basket.

![](img/add_to_stored_basket.png)

In a stored basket the products can be edited, deleted or added to the active shopping cart.

![](img/stored_basket_2.png)

### Checkout

The checkout is set up on one page and offers 5 steps.

silver.eShop

Step 1: A user can register as a new customer or he can buy as a guest.

Customers with an existing login can login and proceed to the checkout process

![](img/check_out_1.png)

Advanced version only

Step 1: A user can register as a new customer, existing customer or he can buy as a guest.

Customers with an existing login can login and proceed to the checkout process

![](img/Checkout.png)

### Invoice and delivery address

Step 2: Invoice address needs to be filled out by new customers or guests ordering and will be prefilled for logged in customers.

![](img/invoice_adress.png)

Step 3: For delivery address the user can either choose the invoice address, one of the addresses from his address book or enter a new delivery address. For Advanced version only the offered delivery addresses will be fetched from the ERP.

![](img/image2019-1-20_17-28-30.png)

### Delivery options

Step 4: The shipping methods and costs can be setup by a configuration.

![](img/checkout_4.png)

### Payment options

The eCommerce solution offers a plugin for PayPal electronic payment (PayPal express). The payment is based on a standard Symfony Bundle. An eZ Partner can integrate other payment providers.

### Order overview

Step 5: The last step summarizes the order. Checkboxes for accepting T&C, Cancellation policy and data protection documents are offered.

![](img/image2018-4-4_18-35-31.png)

### Order

The order is stored in the shop. A confirmation e-mail is sent to the customer and the email address which is set in the configuration.

Advanced version only

The order is forwared to the ERP system and also stored in the shop. A confirmation e-mail is sent to the customer and the email address which is set in the configuration.

## Order History

### The order history

In silver.eShop the order history allows to search for orders from the past.

![](img/image2018-4-4_18-57-10.png)

Advanced version only

In the advanced version the order history allows to search for orders, invoices, delivery note and credit memo from the ERP. The documents for the chosen dates are fetched in real time.

A customer can reorder products easily:

![](img/image2018-5-31_19-36-22.png)
