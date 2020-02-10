#  Bestseller - FAQ 

Can the amount of displayed bestsellers be changed?

The amount of displayed bestsellers can be configured in the eZ Commerce configuration (backend):

![](attachments/23560837/23563972.png)

How can the threshold, from which a product counts as a bestseller, be modified?

The threshold can be configured in the eZ Commerce configuration (backend):

![](attachments/23560837/23563921.png)

How are bestsellers determined?

Existing basket lines from confirmed baskets are counted for each bought product. This is done by the "BasketLineSumDeterminationService".

Where is the bestseller data stored?

The EzBestsellerIndexerPlugin adds a Solr field with the sum of lines of a product. 

    ses_product_ses_sum_of_basket_lines_i

## Attachments:

![](images/icons/bullet_blue.gif) [image2017-3-20 9:7:42.png](attachments/23560837/23563972.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2017-3-22 16:10:30.png](attachments/23560837/23563921.png) (image/png)  
