# Customer Center - Cookbook

## How to extend the customer center forms?

Let's say you need to add more fields to the customer center forms because you want to know to 'Cost center' of the user. This cost center will be stored in eZ and could be sent to the ERP during the order process. Then you need to add this field as dynamic field to your form configuration.

!!! note

    This tutorial will just explain how to extend the form with additional data, that will be stored in eZ. Sending of this additional data (e.g. in the order process) is part of a different process, that needs to be implemented separately. See [Sending additional data in the order.](../../erp_integration/checkout_order/order_submission/order_submission.md)

### Steps:

1.  Extend your User class in eZ with a new field 'cost\_center'. The field type must be one of the supported field types: Text line, Float, Integer, Checkbox.
2.  Add this new field to the form configuration as a dynamic field, first choose the form field type, how the field should be rendered (<http://symfony.com/doc/current/reference/forms/types.html>).
3.  Add validation and other settings. The options are depending on the field type, that you have choosen.
    
    Example: type: *text*, see settings: <http://symfony.com/doc/current/reference/forms/types/text.html>
    
    ``` yaml
    siso_customer_center.default.form.request_user:
       ...
        attributes:
            dynamic:
                cost_center:
                    type: text
                    options:                    
                        constraints:
                            Symfony\Component\Validator\Constraints\NotBlank:
    ```

4.  The new field will appear in your form and can be edited. The data will be stored in the eZ field. If you have extended the request form, the new field might be sent to ERP also, when new contact is created in ERP.
        
!!! warning
        
    The eZ field identifier and the field identifier in the configuration must match, if you want to store data in eZ\!

## How to write a new form data processor?

If you need a special process to be started after one of the form was submitted, you need to write a new data processor. Let´s say you need to update the contact data in ERP, after you have edited the user in the shop.

Condition for this recipe is, that you have prepared a message 'updateContact' to update the contact data in ERP. If you don´t know how to create a message, that will be sent to ERP, see our [tutorials](../../erp_integration/erp_communication/guides/creating_a_new_erp_message/create_project_specific_message.md).  

### Steps:

1.  Create a new form processor, that implements the [FormProcessorInterface](customer_center_api/formprocessorinterface.md).

    **Example**

    ``` php
    class UpdateContactInErpProcessor implements FormProcessorInterface
    {
        public function execute(Form $form, array $lastResult = array())
        {    
            try {
                $updateContactMessage = $this->messageInquiry
                    ->inquireMessage(UpdateContactFactoryListener::UPDATECONTACT);
            } catch(MessageInquiryException $messageException) {
               //TODO handle exception
            }
    
            if (!$updateContactMessage instanceof UpdateContactMessage) {
               //TODO handle exception
            }
    
            /** @var UpdateContact $updateContactMessage */
            $updateContactRequest = $updateContactMessage->getRequestDocument();
            
            //TODO set request data
    
            try {
                $updateContactResponse = $this->transport->sendMessage($updateContactMessage)->getResponseDocument();
    
                if (!$updateContactResponse instanceof ResponseUpdateContact) {
                    //TODO handle missing response
                }
                
                //TODO if required set some response data into last result
                //Example: $lastResult['updateContact'] = $updateContactResponse->status;
    
            } catch (\RuntimeException $rtException) {
                //TODO handle exception
            }    
    
            return $lastResult;
        }
    }
    ```

2.  Define your processor as a service

    ``` 
    <parameter key="siso_customer_center.processor.update_contact_in_erp.class">Project\Bundle\MyProjectBundle\Service\Forms\UpdateContactInErpProcessor</parameter>
    
    <service id="siso_customer_center.processor.update_contact_in_erp" class="%siso_customer_center.processor.update_contact_in_erp.class%">    
    </service>
    ```

3.  Add your data processor to the form configuration

    ``` yaml
    siso_customer_center.default.form.edit_user:
        invalidMessage: error_message_customer_center_forms
        validMessage: success_message_customer_center_edit_user
        #this initialFormValuesService will prefill the form
        initialFormValuesService: siso_customer_center.initial_edit_user_values_service
        formProcessors:
            - siso_customer_center.processor.store_user_form_in_ez
            - siso_customer_center.processor.update_user_roles
            - siso_customer_center.processor.save_profile_in_session
            - siso_customer_center.processor.update_contact_in_erp
    ```

# How to extend the budget with budget per year?

Let´s say you need to prove the user budget also per year. Then you should extend the Ez User class first.

Edit your user class and add a new attribute of type 'Float'. The field type must be one of the supported field types: Text line, Float, Integer, Checkbox.

![](../../img/customer_center_cookbook_1.png)

Name the attribute and add a unique identifier, e.g. budget\_year.

![](../../img/customer_center_cookbook_2.png)

Then you can assign the user budget per year to some users.

![](../../img/customer_center_cookbook_3.png)

## What needs to be extended in the shop?

By default only the budget per order and budget per year are checked by the shop. The [OrderBudgetService](customer_center_api/orderbudgetservice.md) is used to check the budget service.

The easiest way is to override this service in your project and consider also the budget per year in the interface methods.

### How to override the service?

``` 
<parameter key="siso_customer_center.budget_service.order.class">CompanyName\Bundle\ProjectBundle\Service\OrderBudgetService</parameter>

<service id="siso_customer_center.budget_service.order" class="%siso_customer_center.budget_service.order.class%">
    <argument type="service" id="ezpublish.api.repository" />
    <argument type="service" id="silver_basket.basket_repository" />
    <argument type="service" id="silver_common.logger"/>
    <argument type="service" id="silver_trans.translator" />
    <argument type="service" id="ses.customer_profile_data.ez_erp" />
    <argument type="service" id="request_stack"/>
</service>
```

##### Implementation example:

`CompanyName\Bundle\ProjectBundle\Service\OrderBudgetService`

``` php
public function isBudgetExceeded($amount, $userId)
{
    $userService = $this->ezRepository->getUserService();
    try {
        $user = $userService->loadUser($userId);

        $budgetPerOrder = $user->getFieldValue(self::BUDGET_PER_ORDER_IDENTIFIER);
        $budgetPerMonth = $user->getFieldValue(self::BUDGET_PER_MONTH_IDENTIFIER);
        $budgetPerYear = $user->getFieldValue(self::BUDGET_PER_YEAR_IDENTIFIER);

        if ($budgetPerOrder instanceof FloatValue) {
            $isBudgetPerOrderExceeded = $this->isBudgetPerOrderExceeded($amount, $budgetPerOrder);
            ...
        }
        if ($budgetPerMonth instanceof FloatValue) {
            $isBudgetPerMonthExceeded = $this->isBudgetPerMonthExceeded($userId, $amount, $budgetPerMonth);
            ...
        }
        if ($budgetPerYear instanceof FloatValue) {
            $isBudgetPerYearExceeded = $this->isBudgetPerYearExceeded($userId, $amount, $budgetPerMonth);
            if ($isBudgetPerMonthExceeded) {
                $message = 'budget per year exceeded';            
                throw new BudgetExceededException($message);
            }
        }

    } catch(NotFoundException $e) {
        $this->logger->addError('BudgetService: Exception catched: ' . $e->getMessage());
    }

    return false;
}

/**
 * checks if the orders from last year plus given amount crosses the yearly budget
 * 
 */
protected function isBudgetPerYearExceeded($userId, $amount, FloatValue $budget)
{
    //value 0 means that budget is not set
    if ($budget->value == 0) {
        return false;
    }

    //get user orders from the last year
    $orders = $this->basketRepository->getUserOrdersFromLastYear($userId);
    $sumLastYear = $amount;

    if (is_array($orders)) {
        foreach($orders as $order) {
            if (array_key_exists('totalsSumGross', $order)) {
                $sumLastYear += $order['totalsSumGross'];
            }
        }
    }

    return $budget->value < (float) $sumLastYear;
}
```
