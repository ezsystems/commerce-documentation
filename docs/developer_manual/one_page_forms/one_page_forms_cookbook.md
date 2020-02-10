#  One-page forms - Cookbook 

## How to implement a new one-page form?

The best way to make yourself known with the concept is to implement a new one-page form. Let´s say, you want to implement a form, that enables the user to order the product catalog in a printed form.

User will input his email address and confirm, that he wants to order the catalog. Afterwards an email should be send to the shop administrator.

### Part 1 - Build the form components

The basic form components have to be build in the first step:

  - Form entity
  - Form type
  - Form template

#### Form entity

Form entity is just a simple class that holds the data. No business logic is defined here. This is the place where you have to define:

  - form attributes
  - [validation](One-page-forms---API_23560842.html)

Every form entity has to extend the *AbstractFormEntity.*

``` 
namespace Company\Bundle\ProjectBundle\Form;

use Silversolutions\Bundle\EshopBundle\Entities\Forms\AbstractFormEntity;
use Symfony\Component\Validator\Constraints as Assert;
use Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints as SesAssert;

class OrderCatalog extends AbstractFormEntity
{
    /**
     * @Assert\NotBlank()
     * 
     * @var bool
     */
    protected $orderCatalog;

    /**
    * @Assert\NotBlank()
    * @SesAssert\Email() 
    *
    * @var string
    */
    protected $email;   

    /**
     * @return string
     */
    public function getOrderCatalog()
    {
        return $this->orderCatalog;
    }

    /**
     * @param string $orderCatalog
     */
    public function setOrderCatalog($orderCatalog)
    {
        $this->orderCatalog = $orderCatalog;
    }

    /**
    * @return string
    */
    public function getEmail()
    {
        return $this->email;
    }

    /**
    * @param string $email
    */
    public function setEmail($email)
    {
        $this->email = $email;
    }
}
```

#### Form type

Form type defines which form attributes will be rendered and how, you can define other data here, like labels, css classes, data attributes and much more.

It is recommended to define this type as a service, so you can inject any logic that you need.

``` 
namespace Company\Bundle\ProjectBundle\Form\Type;

use eZ\Publish\Core\MVC\ConfigResolverInterface;
use Silversolutions\Bundle\TranslationBundle\Services\TransService;
use Siso\Bundle\ToolsBundle\Service\CountryServiceInterface;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Form\FormInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;
use Symfony\Component\Validator\Constraint;

class OrderCatalogType extends AbstractType
{
    /**
    * Dependency to the silversolutions translation service.
    *
    * @var \Silversolutions\Bundle\TranslationBundle\Services\TransService
    */
    protected $transService;

    /**    
     * @param TransService $transService   
     */
    public function __construct(       
        TransService $transService       
    ) {       
        $this->transService = $transService;       
    }

    /**
     * Builds the form with all fields in required type and sets fields options
     *
     * @param FormBuilderInterface $builder
     * @param array $options
     */
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder  
            ->add('email', 'text', array(               
                'required' => true,
                'label' => $this->transService->translate('email')
            ))  
            ->add('orderCatalog', 'checkbox', array(
                'required' => true,
                'label' => $this->transService->translate('orderCatalog')
            ))
        ;
    }

    /**
     * configure form options
     *
     * @param OptionsResolver $resolver
     * @return void
     *
     */
    public function configureOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults(array(
            'validation_groups' => function(FormInterface $form) {
                $groups = array();
                $groups[] = Constraint::DEFAULT_GROUP;
                return $groups;
            },
        ));
    }

    /**
     * Returns the name of this type.
     *
     * @return string The name of this type
     */
    public function getName()
    {
        return 'company_order_catalog_type';
    }
```

**Service definition** Expand source 

``` 
<service id="company.order_catalog_type" class="Company\Bundle\ProjectBundle\Form\Type\OrderCatalogType">
    <argument type="service" id="silver_trans.translator" />    
</service>
```

#### Form template

Then you need to prepare template, that will render your form. See also [How to render Symfony forms](https://symfony.com/doc/current/form/form_customization.html).

**src/Company/Bundle/ProjectBundle/Resources/views/Forms/order\_catalog.html.twig** Expand source 

``` 
{% extends "SilversolutionsEshopBundle::pagelayout.html.twig"|st_resolve_template %}

{% block content %}
<form action="{{ path('silversolutions_service', {'formTypeResolver': 'order_catalog'}) }}"
  method="post" {{ form_enctype(form) }}>
  {{ form_errors(form) }}  
   
    <div{% if form.email.vars.errors is not empty %} class="error"{% endif %}>
      {{ form_label(form.email) }}
      {{ form_widget(form.email) }}
      {% for error in form.email.vars.errors %}
        <small class="error">{{ error.message }}</small>
      {% endfor %}
    
    <div{% if form.orderCatalog.vars.errors is not empty %} class="error"{% endif %}>
      {{ form_label(form.orderCatalog) }}
      {{ form_widget(form.orderCatalog) }}
      {% for error in form.orderCatalog.vars.errors %}
        <small class="error">{{ error.message }}</small>
      {% endfor %}
    
 {{ form_rest(form) }}
  * {{ 'required fields'|st_translate }}
  <button type="submit" class="button right" name="order_catalog">{{ 'Order Catalog'|st_translate }}</button>
</form>
{% endblock %} 
```

### Part 2 - Prefill the form and implement the processes behind

#### Prefill the form

If you need, you can implement a service, that will pre-fill your form with default data. Therefore the *preDataProcessor* will be used. However, this step is optional.

``` 
namespace Company\Bundle\ProjectBundle\Service\DataProcessor;

use Silversolutions\Bundle\EshopBundle\Services\Forms\DataProcessor\AbstractDataProcessor;
use Silversolutions\Bundle\EshopBundle\Services\CustomerProfileData\CustomerProfileDataServiceInterface;
use Company\Bundle\ProjectBundle\Form\OrderCatalog;

class PreFillOrderCatalogDataProcessor extends AbstractDataProcessor
{
    const SUCCESSFUL_LAST_RESULT_KEY = 'order_catalog';

    /** @var CustomerProfileDataServiceInterface */
    protected $customerProfileDataService;

    public function __construct(
        CustomerProfileDataServiceInterface $customerProfileDataService,    
    ) {
        $this->customerProfileDataService = $customerProfileDataService;        
    }

    /**
     * @param NormalizedEntity $formEntity
     * @param null $lastResult
     * @param Response $response
     * @return mixed|null
     * @throws \Silversolutions\Bundle\EshopBundle\Exceptions\FormDataProcessorException
     */
    public function execute(NormalizedEntity $formEntity, $lastResult = null, Response $response = null)
    {             
        if (!$this->customerProfileDataService->isUserAnonymous()) {

            /** @var OrderCatalog $orderCatalog */
            $orderCatalog = $formEntity->getOriginalForm();

            //prefill form with user email address
            $customerProfileData = $this->customerProfileDataService->getCustomerProfileData();
            $orderCatalog->setEmail($customerProfileData->sesUser->email);
        }        

        return $lastResult;
    }    
} 
```

**Service definition** Expand source 

``` 
<service id="company.pre_data_processor.pre_fill_order_catalog"
         class="Company\Bundle\ProjectBundle\Service\DataProcessor\PreFillOrderCatalogDataProcessor">    
    <argument type="service" id="ses.customer_profile_data.ez_erp" />
</service>
```

#### Implement the processes behind

Implement one or more *dataProcessors*, that will be executed, after the form was submitted.

``` 
namespace Company\Bundle\ProjectBundle\Service\DataProcessor;

use Silversolutions\Bundle\EshopBundle\Entities\Forms\Normalize\Entity as NormalizedEntity;
use Silversolutions\Bundle\EshopBundle\Services\Forms\DataProcessor\AbstractDataProcessor;
use Silversolutions\Bundle\TranslationBundle\Services\TransService;
use Siso\Bundle\ToolsBundle\Service\MailHelperServiceInterface;

class OrderCatalogSendEmailDataProcessor extends AbstractDataProcessor
{
    const SUCCESSFUL_LAST_RESULT_KEY = 'mail_send';
    const SUBJECT_CONTACT_FORM = 'Order catalog e-mail';

    /** @var TransService $translation */
    protected $translation;  

    /** @var MailHelperServiceInterface */
    protected $mailService;

    /**
     * contains mail values from the configuration such as mail receiver or mail sender
     *
     * @var array $mailValues
     */
    protected $mailValues;

    /**
     * @param MailHelperServiceInterface $mailService
     * @param \Silversolutions\Bundle\TranslationBundle\Services\TransService $translation    
     */
    public function __construct(
        MailHelperServiceInterface $mailService,
        TransService $translation       
    ) {
        $this->mailService = $mailService;
        $this->translation = $translation;       
    }

    /**
     * sets mail values from the configuration
     *
     * @param array $mailValues
     */
    public function setMailValues($mailValues)
    {
        $this->mailValues = $mailValues;
    }

    /**
     * @param NormalizedEntity $formEntity
     * @param array|null $lastResult
     * @param Response $response
     * @return mixed|null
     * @throws \Silversolutions\Bundle\EshopBundle\Exceptions\FormDataProcessorException
     */
    public function execute(NormalizedEntity $formEntity, $lastResult = null, Response $response = null)
    {
        try {
            $sender = $this->mailValues['mailSender'];
            $recipient = $this->mailValues['orderCatalogMailReceiver'];

            $this->mailService->sendMailWithRenderedTemplate(
                $sender,
                $recipient,
                self::SUBJECT_CONTACT_FORM,
                'CompanyBundle:Emails:order_catalog.txt.twig',
                'CompanyBundle:Emails:order_catalog.html.twig',
                array(
                    'orderCatalogMailReceiver' => $recipient,
                    'form' => $formEntity->originalForm,
                )
            );           

            $lastResult[self::SUCCESSFUL_LAST_RESULT_KEY] = true;

        } catch (\Exception $e) {            
            $lastResult['_exceptions'][] = 'Error occured when sending the email.';
        }

        return $lastResult;
    }
} 
```

**Service definition** Expand source 

``` 
<service id="company.data_processor.order_catalog_send_email"
         class="Company\Bundle\ProjectBundle\Service\DataProcessor\OrderCatalogSendEmailDataProcessor">
    <argument type="service" id="siso_tools.mailer_helper" />
    <argument type="service" id="silver_trans.translator" />    
    <call method="setMailValues">
        <argument>$ses_swiftmailer;siso_core$</argument>
    </call>
</service>
```

### Part 3 - Bind all together

Create form configuration in order to build the fully functional one-page form.

``` 
parameters:
    ses_forms.configs.order_catalog:
        modelClass: Company\Bundle\ProjectBundle\Form\OrderCatalog
        typeService: company.order_catalog_type
        template: CompanyProjectBundle:Forms:order_catalog.html.twig
        invalidMessage: error_message_order_catalog
        validMessage: success_order_catalog
        preDataProcessor: company.pre_data_processor.pre_fill_order_catalog       
        dataProcessors:
            - company.data_processor.order_catalog_send_email
```

#### Form URL

You can use any of the predefined [*FormsController::formsAction* routes](One-page-forms---Templates_23560838.html) in order to call your form, or even define a new one. In our example (see Form template) we have used following route:

``` 
<form action="{{ path('silversolutions_service', {'formTypeResolver': 'order_catalog'}) }}"
  method="post" {{ form_enctype(form) }}>
```

Call your form by the url and enjoy\!

``` 
/service/order_catalog
```

# reCAPTCHA

## How to activate existing reCPATCHA implementation in forms?

### Generate the reCAPTCHA API-Key pair

Go to: <http://www.google.com/recaptcha/admin>

Login with <developer.silversolutions@gmail.com>. Password is in our wiki.

![](plugins/servlet/confluence/placeholder/unknown-attachment "image2017-2-27 8:56:30.png")

![](plugins/servlet/confluence/placeholder/unknown-attachment "image2017-2-27 8:53:36.png")

![](plugins/servlet/confluence/placeholder/unknown-attachment "image2017-2-27 8:48:27.png")

Register a new domain to get a pair of keys:

![](plugins/servlet/confluence/placeholder/unknown-attachment "image2017-1-18 11:25:9.png")

![](plugins/servlet/confluence/placeholder/unknown-attachment "image2017-1-18 11:24:3.png")

### Configure the reCAPTCHA

Add the generated keys to your parameters.yml:

**parameters.yml**

``` 
ewz_recaptcha_public_key: 6L************************************ev
ewz_recaptcha_private_key: 6L************************************hF
```
 
## How to add reCAPTCHA to a form?

Extend your desired form entity and type like in this examples:

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Entities/Forms/RegisterBusiness.php** Expand source 

``` 
<?php
/**
 * Product silver.e-shop
 *
 * A powerful e-commerce solution for B2B online shops / portals and complex
 * online applications that have access to ERP data, usually in real time.
 * http://www.silversolutions.de/eng/Products/silver.e-shop
 *
 * This file contains the class RegisterBusinessType
 *
 * @copyright Copyright (C) 2013 silver.solutions GmbH. All rights reserved.
 * @license see vendor/silversolutions/silver.e-shop/license_txt_ger.pdf
 * @version $Version$
 * @package silver.e-shop
 */

namespace Silversolutions\Bundle\EshopBundle\Entities\Forms\Types;

use eZ\Publish\Core\MVC\ConfigResolverInterface;
use Siso\Bundle\ToolsBundle\Service\CountryServiceInterface;
use Symfony\Component\Form\FormInterface;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\OptionsResolver\OptionsResolver;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Validator\Constraint;
use Silversolutions\Bundle\TranslationBundle\Services\TransService;
use Symfony\Component\Form\AbstractType;
use EWZ\Bundle\RecaptchaBundle\Form\Type\EWZRecaptchaType;

/**
 * Class RegisterBusinessType
 * Defines the form type for RegisterBusiness form
 */
class RegisterBusinessType extends AbstractType
{
    /** @var array $formsValues */
    protected $formsValues;

    /**
     * Dependency to the silversolutions translation service.
     *
     * @var \Silversolutions\Bundle\TranslationBundle\Services\TransService
     */
    protected $transService;

    /** @var ConfigResolverInterface $configResolver  */
    protected $configResolver;

    /** @var Request */
    protected $request;

    /**
     * @var CountryServiceInterface $countryService
     */
    protected $countryService;

    /**
     * Sets the form values for the selections from the config
     *
     * @param array $formsValues
     */
    public function setFormsValues($formsValues)
    {
        $this->formsValues = $formsValues;
    }

    /**
     * Set dependencies to other services
     *
     * @param TransService $transService
     * @param \eZ\Publish\Core\MVC\ConfigResolverInterface $configResolver
     * @param \Symfony\Component\HttpFoundation\Request $request
     * @param \Siso\Bundle\ToolsBundle\Service\CountryServiceInterface $countryService
     */
    public function setServices(
        TransService $transService,
        ConfigResolverInterface $configResolver,
        Request $request,
        CountryServiceInterface $countryService
    ) {
        $this->transService = $transService;
        $this->configResolver = $configResolver;
        $this->request = $request;
        $this->countryService = $countryService;
    }

    /**
     * build the form with all fields in required type
     * set fields options
     *
     * @param FormBuilderInterface $builder
     * @param array $options
     */
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $salutation = isset($this->formsValues['salutation']) ? $this->formsValues['salutation'] : array();
        $jobRole = isset($this->formsValues['jobRole']) ? $this->formsValues['jobRole'] : array();

        $preferredCountry = $this->configResolver->getParameter('preferred_country', 'ses_forms');

        $builder
            ->add('customerNumber', 'text', array(
                'required' => false,
                'label' => $this->transService->translate('Customer number (if known)') . ': '
            ))
            ->add('company', 'text', array(
                'label' => $this->transService->translate('Company') . ': * '
            ))
            ->add('vatNumber', 'text', array(
                'label' => $this->transService->translate('VAT number') . ': * '
            ))
            ->add('trader', 'checkbox', array(
                'label' => $this->transService->translate('Trader / Retailer') . ': '
            ))
            ->add('tradeRegisterNumber', 'text', array(
                'required' => false,
                'label' => $this->transService->translate('Commercial register number') . ': * '
            ))
            ->add('tradeRegisterCity', 'text', array(
                'required' => false,
                'label' => $this->transService->translate('Commercial register city') . ': * '
            ))
            ->add('tradeRegisterDocument', 'file', array(
                'required' => false,
                'label' => $this->transService->translate('Company registration') . ': * '
            ))
            ->add('sendTradeDocumentPerEmail', 'checkbox', array(
                'required' => false,
                'label' => $this->transService->translate('I would like to submit the documents via email or fax.')
            ))
            ->add('salutation', 'choice', array(
                'label' => $this->transService->translate('Salutation') . ': * ',
                'empty_value' => '-- ' . $this->transService->translate('please choose') . ' --',
                'choices' => $salutation
            ))
            ->add('firstName', 'text', array(
                'label' => $this->transService->translate('First name') . ': * '
            ))
            ->add('lastName', 'text', array(
                'label' => $this->transService->translate('Last name') . ': * '
            ))
            ->add('email', 'email', array(
                'label' => $this->transService->translate('Email') . ': * '
            ))
            ->add('jobRole', 'choice', array(
                'label' => $this->transService->translate('Company role') . ': * ',
                'empty_value' => '-- ' . $this->transService->translate('please choose') . ' --',
                'choices' => $jobRole
            ))
            ->add('street', 'text', array(
                'label' => $this->transService->translate('Street') . ': * '
            ))
            ->add('zip', 'text', array(
                'label' => $this->transService->translate('ZIP code') . ': *',
            ))
            ->add('city', 'text', array(
                'label' => $this->transService->translate('City') . ': * '
            ))
            ->add('county', 'text', array(
                'required' => false,
                'label' => $this->transService->translate('County') . ': '
            ))
            ->add('country', 'country', array(
                'label' => $this->transService->translate('Country') . ': * ',
                'choices' => $this->countryService->getCountryNames($this->getName()),
                'preferred_choices' => array($preferredCountry)
            ))
            ->add('privacyPolicies', 'checkbox', array(
                'label' => $this->transService->translate('privacy_policies') . ' * '
            ))
            ->add('termsAndConditions', 'checkbox', array(
                'label' => $this->transService->translate('terms_conditions') . ' * '))
        ;

        if ($this->newsletterActive()) {
            $builder
                ->add('subscribeNewsletter', 'checkbox', array(
                    'required' => false,
                    'label' => $this->transService->translate('Subscribe to newsletter')
                ));
        }

        $this->addRecaptcha($builder);
    }
    /**
     * Sets default options
     *
     * @param OptionsResolver $resolver
     * @return void
     *
     */
    public function configureOptions(OptionsResolver $resolver)
    {
        $resolver->setDefaults(array(
            'validation_groups' => function (FormInterface $form) {

                $validationGroups = array(Constraint::DEFAULT_GROUP);
                if ($this->isRecaptchaActive()) {
                    $validationGroups[] = 'recaptcha';
                }
                $data = $form->getData();

                if ('IE' != $data->getCountry()) {
                    $validationGroups[] = 'ireland';
                }

                if ($data->getSubscribeNewsletter()) {
                    $validationGroups[] = 'newsletter';
                }

                if ($data->getTrader()) {
                    $validationGroups[] = 'trader';
                }

                if (!$data->getSendTradeDocumentPerEmail()) {
                    $validationGroups[] = 'uploadTradeDocument';
                }

                return $validationGroups;
            },
        ));
    }

    /**
     * Returns the name of this type.
     *
     * @return string The name of this type
     */
    public function getName()
    {
        return 'silver_form_type_business';
    }

    /**
     * Returns true if newsletter module is active.
     *
     * @return bool
     *
     */
    protected function newsletterActive()
    {
        return $this->configResolver->getParameter('newsletter_active', 'siso_newsletter');
    }
    /**
     * Adds recaptcha to the builder if enabled in the configuration
     *
     * @param FormBuilderInterface $builder
     * @return void
     *
     */
    protected function addRecaptcha(FormBuilderInterface $builder)
    {
        if ($this->isRecaptchaActive()) {
            $theme = $this->configResolver->getParameter('recaptcha_options.theme', 'siso_core');
            if (!isset($theme)) {
                $theme = 'light';
            }
            $type = $this->configResolver->getParameter('recaptcha_options.type', 'siso_core');
            if (!isset($type)) {
                $type = 'image';
            }
            $size = $this->configResolver->getParameter('recaptcha_options.size', 'siso_core');
            if (!isset($size)) {
                $size = 'normal';
            }
            $defer = $this->configResolver->getParameter('recaptcha_options.defer', 'siso_core');
            if (!isset($defer)) {
                $defer = true;
            }
            $async = $this->configResolver->getParameter('recaptcha_options.async', 'siso_core');
            if (!isset($async)) {
                $async = true;
            }

            $builder
                ->add('recaptcha', EWZRecaptchaType::class, array(
                    'label' => false,
                    'attr' => array(
                        'options' => array(
                            'theme' => $theme,
                            'type'  => $type,
                            'size'  => $size,
                            'defer' => $defer,
                            'async' => $async,
                        )
                    )
                ));
        }
    }

    /**
     * Returns true if recaptcha is active for given form type
     *
     * @return bool
     *
     */
    protected function isRecaptchaActive()
    {
        $recaptchaValues = $this->configResolver->getParameter('recaptcha', 'siso_core');

        if (is_array($recaptchaValues)
            && array_key_exists($this->getName(), $recaptchaValues)
            && $recaptchaValues[$this->getName()]
        ) {
            return true;
        }

        return false;
    }
}
```

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Entities/Forms/Types/RegisterBusinessType.php** Expand source 

``` 
use EWZ\Bundle\RecaptchaBundle\Validator\Constraints as Recaptcha;

class RegisterBusiness extends AbstractRegistration
{
    /**
     * @Recaptcha\IsTrue(groups={"recaptcha"})
     */
    public $recaptcha;
```

When you have extended the form entity and type you must add a parameter to forms.yml and extend configuration\_core.yml ( silver\_form\_type\_business ):

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/forms.yml** Expand source 

``` 
parameters:
    #recaptcha configuration
    ewz_recaptcha_public_key: public_key
    ewz_recaptcha_private_key: private_key

    siso_core.default.recaptcha_options.theme: light
    siso_core.default.recaptcha_options.type: image
    siso_core.default.recaptcha_options.size: normal
    siso_core.default.recaptcha_options.defer: true
    siso_core.default.recaptcha_options.async: true

    #enable/disable recaptcha per form (name of the form type is required here)
    #pay attention that the public and private key has to be generated in order to use the recaptcha
    #see: http://www.google.com/recaptcha/admin
    #By default the reCAPTCHA must be set to false
    siso_core.default.recaptcha:
        siso_core_contact_type: false
        siso_core_cancellation_type: false
        silver_form_type_private: false
        silver_form_type_business: false
        silver_form_type_activate_business: false
    #recaptcha configuration end
```

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/backend/configuration\_core.yml** Expand source 

``` 
siso_core.default.recaptcha:
    group: core
    type: array
    keys: true
    keys_choices: ['siso_core_contact_type', 'siso_core_cancellation_type', 'silver_form_type_private', 'silver_form_type_business', 'silver_form_type_activate_business']
    choices : [1, 0]

siso_core.default.recaptcha_options.theme:
    group: core
    type: selectbox
    choices: ['light', 'dark']

siso_core.default.recaptcha_options.type:
    group: core
    type: selectbox
    choices: ['image', 'audio']

siso_core.default.recaptcha_options.size:
    group: core
    type: selectbox
    choices: ['normal', 'compact']

siso_core.default.recaptcha_options.defer:
    group: core
    type: boolean

siso_core.default.recaptcha_options.async:
    group: core
    type: boolean
```

  
  
## Attachments:

![](images/icons/bullet_blue.gif) [image2017-2-27 13:17:6.png](attachments/23560845/23563338.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2017-2-27 15:12:0.png](attachments/23560845/23563339.png) (image/png)  
