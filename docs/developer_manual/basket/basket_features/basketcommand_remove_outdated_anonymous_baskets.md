# BasketCommand - remove outdated anonymous baskets

Deletes all anonymous baskets from the DB that are older than validHours.

``` php
// Usage: silversolutions:baskets:clear [validHours]
php bin/console silversolutions:baskets:clear
```

#### Configuration 

`Silversolutions\Bundle\EshopBundle\Resources\config\basket.yml`

``` yaml
parameters: 
    ses_basket.default.validHours: 120
```

The parameter "validHours" can be optionally set as parameter (in hours) - if set as optionally parameter, the config will be ignored.
