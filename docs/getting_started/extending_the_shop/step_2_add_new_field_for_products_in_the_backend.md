#  Step 2 - Add new field for products in the Backend 

What we want to achieve:

The product class already consists all required attributes, like sku or price. However, depending on your business focus, you might want to maintain additional attributes, that are not offered by default.

This can be done in a quite easy way.

## Add additional product data

In the Admin/Content Types area (/admin/contenttypegroup/1/) click on the edit button for the ses\_product content type.

![](attachments/23561032/23571118.png)

One thing that you have to keep in mind, is that the field identifier MUST start with the prefix ***ses*** und you have to use the *snake\_case* convention.

Examples of valid identifiers: ses\_identifier, ses\_book\_author, ses\_copyright\_date

Another thing, that you have to keep in mind is, that not all eZ fields are currently supported to be stored automatically in the product data, see below.  

**Please add 2 new fields to the product:**

Field "ses\_is\_download" (ezboolean):

![](attachments/23561032/23571106.png)

Field "ses\_download\_file" (ezbinaryfile)

![](attachments/23561032/23571107.png)

## Create a new  a product "Refrigerators tuning guide"

 Navigate to a product group (/admin/content/location/253) e.g.

Home / Product Catalog / Major Appliances / Refrigerators 

And click on Create → silver.eshop product

![](attachments/23561032/23571109.png)

**Fill out at least the following fields:**

![](attachments/23561032/23571110.png)

![](attachments/23561032/23571111.png)

![](attachments/23561032/23571112.png)

![](attachments/23561032/23571108.png)

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-02-05 um 08.28.54.png](attachments/23561032/23570972.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-02-05 um 11.38.30.png](attachments/23561032/23570974.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-02-15 um 07.20.57.png](attachments/23561032/23570975.png) (image/png)  
![](images/icons/bullet_blue.gif) [ses\_season.png](attachments/23561032/23570977.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-02-05 um 08.31.17.png](attachments/23561032/23570978.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-02-05 um 08.36.16.png](attachments/23561032/23570981.png) (image/png)  
![](images/icons/bullet_blue.gif) [season.png](attachments/23561032/23570983.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_8-44-23.png](attachments/23561032/23571117.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_8-45-48.png](attachments/23561032/23571118.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_8-47-10.png](attachments/23561032/23571106.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_8-48-25.png](attachments/23561032/23571107.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_8-54-10.png](attachments/23561032/23571109.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_8-57-28.png](attachments/23561032/23571110.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_8-58-5.png](attachments/23561032/23571111.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_8-58-41.png](attachments/23561032/23571112.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_9-1-54.png](attachments/23561032/23571108.png) (image/png)  
