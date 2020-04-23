# Customer Center - FAQ

## Are new shop users synchronized with the ERP as well?

Currently not. This feature can be added. Future versions of the customercenter will provide a feature in order to create or update contacts in the ERP as well.

Currently the customercenter gets a list of contacts assigned to the given customer and displays the list of contacts in the customercenter

## How can i change the customer center forms?

The customer center forms contains two types of fields:

- static fields - can not be removed from the form, some settings (like validation or name) can be configured.
- dynamic fields - can be removed or added, will be read/stored in/from eZ if corresponding identifier exists and can be sent to ERP (request new user).

## Which eZ fields can I add to the User class that will be supported by the customer center?

You can extend your eZ User class with following field types:

- Text line
- Float
- Integer
- Checkbox

If you want to add/edit these fields via customer center forms, you have to add them to the configuration as dynamic fields.

## How can i extend the process after the forms were submitted?

If you need to introduce a special process, after the form was submitted, you will have to write a new data processor. Please see our [cookbook](customer_center_cookbook.md) to find to appropriate recipe.

## How can I add the customer center actions into user menu?

Please check [Component - User Menu](../../cookbook/frontend_customizing/4_front_end_stack_in_details/4.2_customized_foundation_framework/4.2.2_components/component_user_menu.md) or [How to user menu is build?](../../developer_manual/customers/customers_faq.md)
