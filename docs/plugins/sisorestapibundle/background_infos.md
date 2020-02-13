# Background infos 

## Branches

Forlagshuset -  uses develop

Bürkert - uses branch

## Validation rules

src/Siso/RestBundle/Resources/config/validation.yml

validations can be used by siteaccess:

  - ``` 
    siso_rest_api.default.validation
    ```

Groups: always use Default as group in addition

CallBack can be used to implement more complex validations, see validatePartyName() in PartyValidator.php

**Important**:

Validation has to be setup in higher level (e.g. PostalAddress)

``` 
Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\Party:
    properties:
#        PartyIdentification:
#            - NotBlank: ~
        PartyName:
            - Valid: ~
        PostalAddress:
            - Valid: ~

```

## Constraints

Some standard eshop constraints (e,g, Zip) had to be extended in order to work with a form or party object). 

## Translations

Standard Symfony - Are defined in translations/validators.xxx.yml

## Visitors for CatalogElements
## \\Siso\\RestApiBundle\\Service\\InputValidator 

Will assign the error messages directly to the fields in the tree

## Known limitations

  - validation rules have to be defined in own yml, no merging von yml definitions possible\!
  - issue with Composite and workaround
