#  Basket - FAQ 
How long is a basket stored?

It depends if a basket belongs to an anonymous user or a user logged in.

A basket for an anonymous user is stored for 120 hours by default. This can be configured (ses\_basket.default.validHours). A command can be used to clean up anonymous baskets which haven bee expired:

``` 
php bin/console silversolutions:baskets:clear 720
```

A basket for a customer logged in will be stored for ever

What data can be stored in a basket?

 The basket offers a flexible way to store information on header or product level.

A dataMap can be used to store all kind of information. It can be addressed by the PHP API or filled directly using a form:

``` 
 <input type="hidden" name="ses_basket[0][remark]" value="" />
```

How are orders stored?

An order is stored in the basket table as well. The state "confirmed" states that this is an order and has been accepted and forwared to a legacy system such as an ERP.
