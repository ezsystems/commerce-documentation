# Customer center Roles

## Predefined Roles

|Role|Functions||
|--- |--- |--- |
|Customer Center Main Contact|Defines the Role for the main contact for customer center. Only this Role is able to create new customer center and assign Roles to other users.||
|Approver|This Role is assigned to users who can approve orders created by buyers when they exceeded their budget. This Role is used to send out an email during the budget workflow.||
|Buyer|This Role is assigned to users who can order in the shop. They additionally can have a defined budget which they cannot exceed without approval.||

## Defining Roles

Each Role has to be set up in the configuration file:

``` yaml
parameters:
    siso_customer_center.default.main_contact_role: Customer Center Main Contact
    siso_customer_center.default.roles: [Approver, Buyer]
```

- `main_contact_role` defines a main contact Role. There can be more that one main contact per company.
The only limitation is that the user with main contact cannot deactivate their own account.
- `roles` is a list of all available Roles in the Customer center.  

Those predefined Roles should not be changed.

## Setting up a new Role for a user

1. Create new entry in the `siso_customer_center.default.roles` parameter to be able to use it in Customer center.
2. Add a Role to the Back Office.
