# User management

All users which have access to the Customer center are directly assigned to a company.

## Registration and storing users

The shop distinguishes between business and private users. The users are stored in different Locations.

In addition, the Customer center handles business customers differently depending on whether they are using the Customer center or not.

|Use case|Stored in|Note|
|--- |--- |--- |
|A new customer registers and chooses private customer|`/Shop users Demo-Shop/Private users/<new customer>`||
|A new customer registers and chooses business customer||New business users require a validation done by the shop administrator, because they can get better prices. By default they are not stored in the shop immediately and have to wait for a customer number.|
|A business customer activates their account in the shop. No access to the Customer center is planned for this customer in ERP|`/Shop users Demo-Shop/Business users/<new customer>`||
|A business customer activates their account in the shop and is assigned access to the Customer center in ERP|`/Shop users Demo-Shop/Business users/<new company>/<new user>`|If there is no Customer center yet for this customer (customer number is used to check this) the shop attempts to create it, after ensuring that the customer number is not in use. If the user is the main contact (defined in ERP), the shop creates a customer center for this business customer. The user is created inside this Customer center and this user is assigned as a main contact for this company. If the user is not the main contact, an error message is shown to contact main contact person for the company. After that the main contact can create an account for this user|

When a Customer center is created for a given customer number, the Customer center checks if a Company Content item (Content Type `ses_company`) with the given customer number already exists.

## Login process

When a customer logs in, the shop searches for a user with the given email inside the `Demo-Shop` or `Editors` user tree.

For details see [Login and registration](../../login_and_registration/login_and_registration.md).
