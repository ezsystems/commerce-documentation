# FormProcessorInterface

# Introduction

This interface needs to be implemented by every service that is defined in the *formProcessors* key of a form definition. For example in vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/config/customercenter.yml:

``` yaml
siso_customer_center.default.form.add_user:
    ...
    formProcessors:
        - siso_customer_center.processor.validate_customer_and_contact_number
        ...
```

## FormProcessorInterface

|Method|Description|
|--- |--- |
|public function execute(Form $form, array $lastResult = array());|Processes the given form data and MUST return the $lastResult parameter with possible changes/additions|
