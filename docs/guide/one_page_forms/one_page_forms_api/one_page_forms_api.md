# One-page form API

## PreDataProcessor and DataProcessors

The main services for one-page forms services are `preDataProcessor` and `dataProcessors`.
Both implement the `AbstractDataProcessor`, so the services must implement the `execute()` method.

Because every form entity is a custom class, when the form is passed to `AbstractDataProcessor`,
it is encapsulated and passed in a `NormalizedEntity $formEntity`.

Every implementation of the `AbstractDataProcessor` can get and set the form data from/in the original form.
Every implementation must return `$lastResult`.

Every `dataProcessor` can get/update the results of the previous `dataProcessors` that are stored in `$lastResult`.
Every `dataProcessor` can set an additional error message that is displayed for the user if the process is not successful.

``` php
class SendEmailDataProcessor extends AbstractDataProcessor
{
    const LAST_RESULT_IDENTIFIER = 'send_email';

    public function execute(NormalizedEntity $formEntity, $lastResult = null, Response $response = null)
    {
        try {
            // get the original form
            $originalForm = $formEntity->getOriginalForm();
            $company = $originalForm->get('company');
            $originalForm->set('company', $company . '_' . uniqid());

            // get the customer profile data from the previous data processor   
            $customerProfileData = isset($lastResult[CreateCustomerProfileDataDataProcessor::SUCCESSFUL_LAST_RESULT_KEY]) ? 
                                    $lastResult[CreateCustomerProfileDataDataProcessor::SUCCESSFUL_LAST_RESULT_KEY] 
                                    : null;
            if ($customerProfileData instanceof CustomerProfileData) {
                $originalForm->set('customerNumber', $customerProfileData.sesUser.customerNumber);
            }
            
            // send an email with given form data   
            if ($this->sendEmail($originalForm)) {
                $lastResult[self::LAST_RESULT_IDENTIFIER] = true;
            }

        } catch (\Exception $e) {
            // TODO - log the exception

            // if you add a message to the $lastResult['_exceptions'], it will be displayed as an error message for the user
            // therefore a readable error message is usefull
            $lastResult['_exceptions'][] = 'Error occured when sending the email.';
        }

        return $lastResult;
    }
}
```

## Form validators

Besides standard [Symfony validators,](http://symfony.com/doc/current/validation.html)
eZ Commerce offers additional validators.

### ZIP validator

The post code (ZIP) validation rules are different for each country.
To define the different code pattern for each post code a new service is defined.

!!! note

    This validator validates the post code based on the submitted country.
    If you want to use this validator in your project, make sure that your Form contains the `$country` attribute.

``` xml
<parameter key="ses_forms.zip_validator.class">Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints\ZipValidator</parameter>

<!-- ZIP validator service -->
<service id="ses_forms.zip_validator" class="%ses_forms.zip_validator.class%">
    <argument type="service" id="doctrine.orm.default_entity_manager" />
    <tag name="validator.constraint_validator" alias="ses_forms.zip_validator" />
    <call method="setServices">
        <argument type="service" id="ezpublish.config.resolver" />
        <argument type="service" id="silver_trans.translator" />
        <argument type="service" id="silver_common.logger" />
    </call>
</service>
```

The validator takes the pattern by country from the configuration, and compares it with the value in the form.
Country codes, pattern and code rules are defined in `ses_patern_zip.yml`.

``` php
//Check if the value matches with the "POST_CODE_RULE"
$codeRule = '/' . $countryPattern['POST_CODE_RULE'] . '/';
if (preg_match($codeRule, $value)){
    return;
}
```

Example of the `ses_pattern_zip.yml` file:

``` yaml
parameters:
    siso_pattern_zip.default.country:
            -
             CODE: AD
             POST_CODE_PATTERN: AD999
             POST_CODE_RULE: "^AD[0-9]{3}$"
            -
             CODE: AE
             POST_CODE_PATTERN: ""
             POST_CODE_RULE: ".*"
            -
             CODE: AF
             POST_CODE_PATTERN: ""
             POST_CODE_RULE: ".*"
            -
             CODE: AG
             POST_CODE_PATTERN: ""
             POST_CODE_RULE: ".*"
```

This new file must be imported in `silver.eshop.yml`:

``` yaml
- { resource: "ses_patern_zip.yml"}
```

### Email validator

A custom email validator extends the standard Symfony validation.

#### NonExistingEmailValidator

`NonExistingEmailValidator` checks if a user with the given email already exists.

#### User searching

You can set the Location where Users are contained in the following configuration:

``` yaml
ses_ez_helper.default.parent_location_id_users_members: 12
```

### Phone/fax number validator

A phone and/or fax validator verify a valid number format. The folowing formats are supported:

- 0123456789
- +490123456789
- +49-0123456789
- 030-1234567
- 030/1234567
- +4930/1234567
- 0123 456 789 (spaces)

The following regex allows all combinations of 0-9, `+`, `-` and `/`, if minimum and maximum length fit.

``` 
('/^[0-9\-\+\/]{%min%,%max%}$/')
```

You can define the minimum and maximum length for the validator:

``` yaml
ses_phone_validator:
    min: 9
    max: 15
```

### VAT number validator

`VatNumberValidator` checks if the VAT number for commercial customers inside the European Union is valid.
The [VIES web-service (SOAP)](http://ec.europa.eu/taxation_customs/vies/checkVatService.wsdl) of the European Commission is used. The condition for validation is that the VAT number contains the country code.

## Form interfaces

### FormEntityInterface

`FormEntityInterface` (`Silversolutions\Bundle\EshopBundle\Entities\Forms\FormEntityInterface`) is an interface for form entities
which defines a common way to access to the entity data.

### CheckoutFormInterface

`CheckoutFormInterface` (`Siso\Bundle\CheckoutBundle\Api\CheckoutFormInterface`) provides methods that are necessary for all checkout forms.

### CheckoutAddressInterface

`CheckoutAddressInterface` (`Silversolutions\Bundle\EshopBundle\Form\CheckoutAddressInterface`) provides methods that are necessary for forms
that are handling addresses in checkout process.

### AbstractFormEntity

`AbstractFormEntity` (`Silversolutions\Bundle\EshopBundle\Entities\Forms\AbstractFormEntity`) implements the methods of the [FormEntityInterface](#formentityinterface).
