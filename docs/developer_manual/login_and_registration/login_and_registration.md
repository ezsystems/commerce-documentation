#  Login and registration 

## Introduction

The shop uses the standard user management of the CMS system. Users are located under the section /users.

eZ Commerce extended the user management with these new attributes:

  - Customer profile data
  - Customer number
  - Contact number

The attribute Customer profile data is able to store additional information coming from the ERP such as invoice address, buyer address, delivery addresses and other customer related information. 

The customer number and contact number is used to assign a shop user to a customer in the ERP-system.

For each shop a special user group can be used to separate users. It is also possible to share users among different shops.

## Important URLs

  - Login: /login
  - Registration for private customers: /register/private 
  - Registration entry page (Advanced version only): /registration/choice 

## What happens during a login process

What happens during a login process:

  - When a user logs in 
      - the shop checks if the user has a customer number 
          - The customer data  from the ERP will be stored in a field in the eZ User record (Customer profile data). In case of the ERP system is not available the shop has access to the latest data from the ERP-System.
      - the shop checks if the user has a contact number  
          - The contact data from the ERP will be fetched and stored in "Customer profile data" as well
  - When the data is stored in the ERP the shop will take care of the versions created by eZ Platform. eZ Platform does limit the number of versions created in case a user (or object) has been updated. The shop uses a setting to limit the number of versions (silver\_tools.default.versions\_count: 10) and will remove archived versions. If the number of versions to be removed will exceed 20 versions just 20 versions will be removed to avoid that the login process takes to much time (and might lead to a timeout). 

## Login

eZ Commerce provides a flexible login functionality. In the shop a user can login with username or email password. Additionally we can add a field "customer number", which is required for logging into customer center.

The methods "retrieveUser" and "checkAuthentication" of the AuthenticationProvider have been overriden in order to provide the login functionality.

The logic for searching for a user is:

  - Determined by the location id,which is configured by the siteaccess (siso\_core.default.user\_group\_location).
  - Check if a user with given the email address is stored beneath configured location. The search will look in subfolders such as business, private or editors.

## Registration options

The eZ Commerce provides different options for users to register into the shop. 

There are different registration forms for different target groups such as private or business customers.

Private Customer

**Business Customer**

A **private customer** can register directly.

A double opt-in process will check the email address, create and activate the user

A** business customer** has two options to register

1.  **Apply for new account** - fill the business form and apply for an account.   
      
    The shop owner will check the provided data and create a customer record in the [ERP](http://confluence.ng.silverproducts.de/display/EX/Term+-+ERP)system.

<!-- end list -->

1.  **Activate account** - a business customer who already has a customer number can register using a customer number and a invoice number.  
      
    The shop will check this data using a request to the ERP. There are two options:

  - **activate business account** - The customer will be created using his customer number and he will see immediately his special discounts in the shop.

<!-- end list -->

  - **create main contact in Customer Center** - if customer center is enabled your company will be created in the shop. Your account will be created as main contact for it.  
      
    For more details see: [Customercenter and user management](http://confluence.ng.silverproducts.de/display/EX/Customercenter+and+user+management)

The forms and the processes behind the forms can be customized. The shop uses the very flexible form features of the Framework Symfony2.

# Configuration

see [Login and registration - Configuration](Login-and-registration---Configuration_23561081.html)

## Technical infos

**Access control**

You will find Details about how security system work in the document [Access control](Access-control_23560922.html)

**Token**

Details about using the Token system offered by Symfony is document in [TokenController](TokenController_23560947.html)

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

  - Microsoft Dynamics NAV (former navision)
  - Microsoft Dynamics AX  
    
  - SAP

CMS

**CMS**  (content management system) is a software for the collective creation, editing and organization of content   mostly in web pages, but also in other forms of media. This can consist  text and multimedia documents. An author with access rights can such a system, in many cases with little programming or HTML knowledge use, since the majority of systems have a graphical user interface.

*Source*: Wikipedia  

## FAQ

How to extend the login with new fields

A section in the Cookbook describes how to extend the login e.g. with a customer number [Login and registration - Cookbook](https://doc.silver-eshop.de/display/EZC14/Login+and+registration+-+Cookbook)
## Cookbook

In the [cookbook](Login-and-registration---Cookbook_23560279.html) you will find:

  - Login process adaptation in 3 steps
  - How to login from a crontroller
