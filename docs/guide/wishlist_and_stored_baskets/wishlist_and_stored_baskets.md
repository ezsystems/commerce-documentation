# Wishlist and Stored Baskets

## Introduction

eZ Commerce offers feature to save your favorite products in a list, so the customer can easily access them or add them to his cart. There are 2 options for storing: 

- **Wishlist** 
- custom named lists called **Stored Basket**.

This feature is available only for the customers that have an account in eZ Commerce and are logged in.

The difference between Wishlist and Stored Basket is that there is only one wishlist per user, but there can be many stored baskets with different names for one user. 

Comparison table: 

| Feature                | Wishlist    | Stored basket |
| ---------------------- | ------- | ------- |
| More than one list     | NO | YES           |
| Add products to basket | YES  | YES           |
| Store the quantity     | NO | YES           |
| Give list a name       | NO | YES           |
| Show prices            | NO | YES           |

The wishlist and stored basket function is based on the basket system of eZ Commerce. 

## Before you start 

Please keep in mind that Wishlist/Stored Baskets is really connected with different modules in our shop. Be sure to check these out:

- [Basket](../basket/basket.md)
- [BasketService](../basket/basket_api/basketservice.md)

The stored basket feature can be deactivated in a config file:

``` yaml
twig:
    globals:
        stored_baskets_active: false
```
