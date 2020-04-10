# Form interfaces

## FormEntityInterface

!!! note "Namespace"

    `Silversolutions\Bundle\EshopBundle\Entities\Forms\FormEntityInterface`

FormEntityInterface is an interface for form entities, which defines a common way to access to the entity data.

|Method|Parameters|Usage|
|--- |--- |--- |
|get|$key|get access to all private attributes, e.g. for the normalizer|
|set|$key, $value|get access to set all private attributes, e.g. for the normalizer|
|hasChanged||returns true if the form values have been changed after the form was submitted|
|setChanged|bool $changed|sets the form changed flag|

## CheckoutFormInterface

!!! note 'Namespace"

    `Siso\Bundle\CheckoutBundle\Api\CheckoutFormInterface`

Provide methods that are necessary for all checkout forms.

|Method|Parameters|Usage|
|--- |--- |--- |
|setForceStep|$forceStep|sets the force step value|
|getForceStep||gets the force step value|

## CheckoutAddressInterface

!!! note "Namespace"

    `Silversolutions\Bundle\EshopBundle\Form\CheckoutAddressInterface`

Provide methods that are necessary for forms that are handling addresses in checkout process.

|Method|Parameters|Usage|
|--- |--- |--- |
|setAddressSecond|$addressSecond|sets the address second|
|getAddressSecond||gets the address second|
|setCity|$city|sets the city|
|getCity||gets the city|
|setCompany|$company|sets the company|
|getCompany||gets the company|
|setCountry|$country|sets the country|
|getCountry||gets the country|
|setCounty|$county|sets the county|
|getCounty||gets the county|
|setEmail|$email|sets the email address|
|getEmail||gets the email address|
|setCompanySecond|$name|sets the company second|
|getCompanySecond||gets the company second|
|setPhone|$phone|sets the phone number|
|getPhone||gets the phone number|
|setStreet|$street|sets the street|
|getStreet||gets the street|
|setZip|$zip|sets the zip number|
|getZip||gets the zip number|
