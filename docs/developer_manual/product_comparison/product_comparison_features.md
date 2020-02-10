#  Product comparison - Features 

### Who is able to use comparison feature

The comparison feature is available for every user. There can be many comparison lists for all users including unregistered (anonymous) browser sessions. Some service is advisable, which frequently cleans *old* anonymous comparison lists for heavy-loaded sites.

### How to add product to comparison list

Only orderable products can be added to comparison.<span class="Apple-tab-span"> If a product is a variant, then the compare-button is only shown when all characteristics for this variant are selected.

There are 2 places for adding a product to the comparison list.

#### Product Detail page

Add to comparison list link is placed in product detail page in the right column. See below:

![](attachments/23560678/23563513.png)

#### Product Listing page

Add to comparison list link is placed in the middle of product information. See below:

![](attachments/23560678/23563517.png) 

If the product is already in the comparison list it is grey and tooltip informs about no ability to add to comparison list.

![](attachments/23560678/23563241.png)

### Messages that are shown when adding to comparison

After a successful adding to the comparison list, a notification message is displayed.

![](attachments/23560678/23563519.png)

Every item can be stored only once. A notice is displayed if a user tries to add an item twice:

![](attachments/23560678/23563515.png)

When adding products, **no prices** and **no quantity** is stored in the comparison list.

### Where to find the link to the comparison list

The comparison can be found in the header of the website and in the profile page in right user menu. See below:

#### Header

In the header section the link shows the number of products in the comparison list.

When a user hovers over the button, the sub-menu is displayed with all types of the comparison lists and their numbers of products per list.

When adding or removing products to or from comparison lists, the page-header information is updated.

![](attachments/23560678/23563239.png)

#### Profile

The profile page provides a simple link in the right side-navigation.

![](attachments/23560678/23563243.png)

### Comparison list overview page

![](attachments/23560678/23563242.png)

#### How products are grouped into categories

One group contains of one kind of products (like speakers, notebooks etc). It is determined by internal shop logic and can be overridden in projects.

#### How attributes are displayed

Attributes (or product specifications) are grouped for display and can be opened and closed by clicking the arrows in their header rows (accordion UI).

The comparison feature provides the ability to show/hide attribute-groups and attributes if they are identical. 

1.  Product groups of attributes can be collapsed by default when all contained attributes are identical  
    There is a configuration parameter, which determines if eZ Commerce should hide groups, which have solely identical attributes for compared products  
    
2.  Product attributes can be hidden when the are identical  
    There is a configuration parameter, which determines if eZ Commerce should hide attributes, which have an identical value 

#### Sorting of the basket

Customers can move or rearrange products in the comparison list using drag & drop functionality (the arrow-cross above the product image in the list is clickable)

After drag and drop, the position in the list will be stored in the current [comparison-basket](Product-comparison---API_23560693.html#Productcomparison-API-Baskettype). In the next call customer will see sorted list by his preference.

#### Slider for more products in the list

If there are more than 3 products in the list, a slider or page navigation is shown. The customer can use it to scroll through the products in the list.

The slider shows Information about the currently viewed list section and the total number of products in the list.

#### Removing products and lists

There are 2 ways of removing items from the comparison list:

  - First one is to delete the whole list by clicking on the 'Delete' link in the top left box. It will delete the currently displayed list.
  - The second option is deleting one product from the list by clicking on the trash-can icon in the top right corner of the product box. 

#### Public comparison lists

There are public lists which have their own URL (like http://\<site-host\>/comparison/best-notebooks), which are created and maintained by a shop-administrator. They cannot be removed nor changed by customers.

Public comparison lists are useful for example for promotion campaigns.

#### Product is not available

If the product is not in the shop's catalog any longer, it will be removed from the comparison list, implicitly, and a notification message will appear. The removal will only occur with an HTTP request to the respective comparison list. There is no background process which will remove all non-existent products from all lists.

#### Print functionality

Comparison lists can be printed by clicking the respective link in the left sidebar. The link will print the current page immediately without showing a print-optimized view.  

## Attachments:

![](images/icons/bullet_blue.gif) [add\_product\_to\_comparison.png](attachments/23560678/23563513.png) (image/png)  
![](images/icons/bullet_blue.gif) [add\_to\_comparison\_success.png](attachments/23560678/23563519.png) (image/png)  
![](images/icons/bullet_blue.gif) [add\_to\_comparison\_notice.png](attachments/23560678/23563515.png) (image/png)  
![](images/icons/bullet_blue.gif) [add\_product\_list1.png](attachments/23560678/23563517.png) (image/png)  
![](images/icons/bullet_blue.gif) [add\_product\_list2.png](attachments/23560678/23563241.png) (image/png)  
![](images/icons/bullet_blue.gif) [link\_comparison\_header.png](attachments/23560678/23563239.png) (image/png)  
![](images/icons/bullet_blue.gif) [link\_comparison.png](attachments/23560678/23563243.png) (image/png)  
![](images/icons/bullet_blue.gif) [FireShot Capture 67 - Produktvergleich - http\_\_\_harmony.local\_comparison\_\#5381.png](attachments/23560678/23563260.png) (image/png)  
![](images/icons/bullet_blue.gif) [FireShot Capture 67 - Produktvergleich - http\_\_\_harmony.local\_comparison\_\#5381.png](attachments/23560678/23563242.png) (image/png)  
