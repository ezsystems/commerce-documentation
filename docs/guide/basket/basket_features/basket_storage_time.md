# Basket storage time

The time basket is stored depends on whether a basket belongs to an anonymous user or a user logged in.

A basket for a logged-in customer is stored for ever.

A basket for an anonymous user is stored for 120 hours by default.
You can configure a different value:

``` yaml
parameters: 
    ses_basket.default.validHours: 120
```

You can use the `silversolutions:baskets:clear` command to clean up anonymous baskets which have been expired:

``` bash
php bin/console silversolutions:baskets:clear <validHours>
```

It deletes all anonymous baskets from the database that are older than `validHours`.

For example

``` bash
php bin/console silversolutions:baskets:clear 720
```
