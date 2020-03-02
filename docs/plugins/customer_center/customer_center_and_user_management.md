# Customer Center and user management

# Introduction

The users will be stored in the User section of the system.

The following screenshot shows the users assigned to the Demo-Shop:

![](../img/customer_center_and_user_mgmt_1.png)

The brown marked structure is introduced by the **Customer Center**. All users which have a customer center are directly assigned to a company.

Used Content-Types in Z Publish:

|Element|Contenttype|
|--- |--- |
|![](../img/customer_center_and_user_mgmt_2.jpg)|User group|
|![](../img/customer_center_and_user_mgmt_3.jpg)|Company|
|![](../img/customer_center_and_user_mgmt_4.jpg)|User|

## Registration and where are users stored

The shop distinguishes between business and private users. The users are stored in different locations.

In addition the customer center handles business customer differently, if they are using a customer center or not.

|Use case|Stored in|Note|
|--- |--- |--- |
|A new customer registers and chooses private customer|/Shop users Demo-Shop/Private users/<new customer>||
|A new customer registers and chooses business customer||New business users require a validation done by the shop administrator, since they might get better prices.</br>So by default they are not stored in the shop immediately and have to wait for a customer number.|
|A business customer activates himself in the shop and for this customer no customer center is planned in the ERP|/Shop users Demo-Shop/Business users/<new customer>||
|A business customer activates himself in the shop and for this customer a customer center is assigned in the ERP|/Shop users Demo-Shop/Business users/<new company>/<new user>|If there is no customer center yet for this customer (Custmer Number is used to check this) the shop will try to create it, after checking Customer Number is not already in use.</br>If the user is the MainContact (Defined in ERP):</br>- The shop will create a customer center for this business customer.</br>- The user will be created inside this customer center and this used will assigned as a main contact for this company.</br>If the user is not MainContact (Defined in ERP):</br>- Error message is shown to contact main contact person for the company.</br>- After that the main contact person can create an account for this user|

When a customercenter for a given customer number shall be created the customercenter will check if a company object (Contenttype ses\_company) with the given customer number is already created.

## Login process

When a customer logs in, the shop searches for a user with given email inside the "Demo-Shop" or "Editors" user tree.

For details see [Login and registration](Login-and-registration_29819127.html)
