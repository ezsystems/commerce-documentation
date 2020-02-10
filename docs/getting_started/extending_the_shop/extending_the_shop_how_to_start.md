#  Extending the shop - how to start 

This chapter describes how to implement a feature in eZ Commerce. We will cover some of the common uses cases required for an eCommerce solution. 

More information about how to extend eZ Commerce can be found in the detail pages of the documentation.

The examples will step-by-step add a new feature "Selling digital products" to eZ Commerce. We will

  - Add new fields to the product content type
  - Display the fields in the product detail page
  - Change the basket preview page
  - Add some business logic to deliver a digital product via email after a customer has successfully placed an order

The repo used for the getting started tutorial is placed in the vendor section of the installation.

Starting from Version 3.4 Symfony recommends not to use bundles any more for templates and logic which belongs to you application.

Templates or configuration can be overidden in app/config or app/Ressources/views

## Step 1 - Create a new DemoBundle

The new bundle will contain configuration, templates and some php classes.

## Step 2 - Add new fields to a product

This example describes howto add a new field and how to use it in a product

## Step 3 - Display the new fields on the products detail page

## Step 4 - Change the basket preview page

## Step 5 - Deliver the product to the customer
