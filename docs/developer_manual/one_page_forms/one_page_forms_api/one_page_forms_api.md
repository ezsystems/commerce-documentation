#  One-page forms - API 

## PreDataProcessor and DataProcessors

The most important services are the ***preDataProcessor*** and ***dataProcessors***. Both of them must implement the *AbstractDataProcessor,* therefore the services must implement the *execute()* method.

**AbstractDataProcessor**

``` 
namespace Silversolutions\Bundle\EshopBundle\Services\Forms\DataProcessor;

abstract class AbstractDataProcessor implements DataProcessorInterface
{
    ...
}
```

``` 
namespace Silversolutions\Bundle\EshopBundle\Services\Forms\DataProcessor;

use Symfony\Component\HttpFoundation\Response;
use Silversolutions\Bundle\EshopBundle\Entities\Forms\Normalize\Entity as NormalizedEntity;

/**
 * Defines the interface of all DataProcessors.
 *
 */
interface DataProcessorInterface
{
    /**
     * @param \Silversolutions\Bundle\EshopBundle\Entities\Forms\Normalize\Entity $formEntity
     * @param null $lastResult
     * @param Response $response
     * @return mixed
     */
    public function execute(NormalizedEntity $formEntity, $lastResult = null, Response $response = null);
}
```

### What the services can/should do?

Since every form entity is a custom class, when the form is passed to the *AbstractDataProcessor*, it is encapsulated and passed in a *NormalizedEntity $formEntity*.

  - Every implementation of the *AbstractDataProcessor* can get and set the form data from/in the original form.
  - Every implementation of the *AbstractDataProcessor* has to return the $lastResult
  - Every dataProcessor can get/update the results of the previous dataProcessors, that are stored in the $lastResult
  - Every dataProcessor can set additional error message, that will be displayed for the user, if the process was not successful  

**Example: SendEmailDataProcessor** Expand source 

``` 
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

Next to the standard [Symfony validators](http://symfony.com/doc/current/validation.html), eZ Commerce offers some additional validators.

### ZIP Validator

A custom ZIP validator has been implemented in order to extend the standard Symfony validation.

The post code - ZIP - validation rules are different for each country. To define the different code pattern for each post code a new service is defined.

Please notice, that this validator validates the post code based on the submitted country. If you want to use this validator in your project, make sure, that your Form contains an attribute:

    $country

New Service ZIP Validator

**EshopBundle/Resources/config/ses\_services.xml**

``` 
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

To validate the ZIP code it takes the pattern by country from the configuration, and compare with the value in the form. In the file "ses\_patern\_zip.yml" are define the country codes, the pattern and the code rule\!

So in the "*ses\_forms.zip\_validator*" service it checks that values (See code.)

``` 
//Check if the value matches with the "POST_CODE_RULE"
$codeRule = '/' . $countryPattern['POST_CODE_RULE'] . '/';
if (preg_match($codeRule, $value)){
    return;
}
```

Example of the *ses\_pattern\_zip.yml* YML file:

**EshopBundle/Resources/config/ses\_patern\_zip.yml** Expand source 

``` 
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

This new file is include in silver.eshop.yml file:

``` 
- { resource: "ses_patern_zip.yml"}
```

### Email Validator

A custom email validator has been implemented in order to extend the standard Symfony validation.

``` 
use Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints as SesAssert;
 
/**
 * @SesAssert\Email()
 */
protected $email;
```

### NonExistingEmail Validator

A custom validator has been implement, that checks if user with given email already exists.

``` 
use Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints as SesAssert;
 
/**
 * @SesAssert\NonExistingEmail()
 */
protected $email;
```

####   
User searching

It is possible to manage the searching for a user in a given location. The locationId must be set in a configuration file.

``` 
ses_ez_helper.default.parent_location_id_users_members: 12
```

### Phone/fax number Validator

A phone and/or fax validator has been implemented to verify a valid number format. These formats are supported:

  - 0123456789

  - \+490123456789

  - \+49-0123456789

  - 030-1234567

  - 030/1234567

  - \+4930/1234567

  - 0123 456 789 (spaces)

Rule: All combination of 0-9, +, - and / is possible, if minimum and maximum length (see section Configuration) fits.

**regex**

``` 
('/^[0-9\-\+\/]{%min%,%max%}$/')
```

``` 
use Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints as SesAssert;
 
/**
 * @SesAssert\Phone()
 */
protected $phoneNumber;
```

#### Configuration

You can define the min and max length for the validator:

``` 
ses_phone_validator:
    min: 9
    max: 15
```

### VatNumber Validator

A custom validator has been implemented to check if the VAT number for commercial customers inside the European Union is valid. For the validation the VIES web-service (SOAP) of the European Commission is used. The condition for validation is that the vat number contains the country code.

##### Location of the VIES Webservice

``` 
http://ec.europa.eu/taxation_customs/vies/checkVatService.wsdl
```

``` 
use Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints as SesAssert;
 
/**
 * @SesAssert\VatNumber()
 */
protected $vatNumber;
```

## reCAPTCHA

The EWZRecaptchaBundle is used, it provides reCAPTCHA form field for Symfony.

An additional validator is provided by EWZRecaptchaBundle

``` 
use EWZ\Bundle\RecaptchaBundle\Validator\Constraints as Recaptcha;
 
/**
 * @Recaptcha\IsTrue(groups={"recaptcha"})
 */
public $recaptcha;
```
