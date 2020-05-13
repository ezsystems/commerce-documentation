# Bestseller FAQ

## Can the number of displayed bestsellers be changed?

The number of displayed bestsellers can be configured in the eZ Commerce configuration (Back Office):

![](../img/bestseller_3.png)

## How can I change the point from which a product counts as a bestseller?

The threshold can be configured in the eZ Commerce configuration (Back Office):

![](../img/bestseller_4.png)

## How are bestsellers determined?

`BasketLineSumDeterminationService` counts existing basket lines from confirmed baskets for each product that was bought.

## Where is the bestseller data stored?

`EzBestsellerIndexerPlugin` adds a Solr field with the sum of lines of a product. 

`ses_product_ses_sum_of_basket_lines_i`
