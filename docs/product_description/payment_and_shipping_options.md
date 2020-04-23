# Payment and shipping options

## Configuration of payment and shipment options

Next to standard payment method invoice an interface to Paypal Express is offered.
Payment providers Ogone/Ingenico and Telecash can be implemented on request.

The payment is based on a standard Symfony Bundle. An eZ Partner can integrate other payment providers.
 
![](img/image2018-5-31_19-50-35.png)

The payment and shipping method offered can be enabled or disabled per siteaccess.

A more complex logic can be implemented by partners.

## Shipping costs

silver.eShop and silver.eShop Advanced offer a flexible way to define shipping costs, if this is not set in the ERP system.

Shipping costs can be setup per

- shop (silver.eShop offers one shop only)
- currency
- shipping method
- country
- state
- zip code (e.g. for exceptions such as delivery to islands)

In addition it is possible to configure different shipping cost depending on the amount of the basket (including free of freight rules) :

If no shipping costs are defined for a given country, shipping method, currency and value of goods a fallback cost from the configuration is used.

![](img/shipping_costs.png)
