# Checkout functions

## getBasketPaymentMethods

### Resourcename

/api/ezp/v2/siso-rest/checkout/payment-methods (GET)

### Summary

returns available payment options for current basket

Parameter based default implementation can be replaced via overridden service ID

standard service id: 'siso_rest_api.payment_methods_service'

### Request

empty

### Response

```
{
    "PaymentMethodDataResponse": {
        "_media-type": "application\/vnd.ez.api.PaymentMethodDataResponse+json",
        "paymentMethods": {
            "paypal": "paypal",
            "invoice": "invoice"
        },
        "defaultMethod": "invoice"
    }
}
```

## getBasketShippingMethods

### Resourcename

/api/ezp/v2/siso-rest/checkout/shipping-methods (GET)

### Summary

returns available delivery options for current basket

Parameter based default implementation can be replaced via overridden service ID

standard service id: 'siso_rest_api.shipping_methods_service'

### Request

empty

### Response

```
{
    "ShippingMethodDataResponse": {
        "_media-type": "application\/vnd.ez.api.ShippingMethodDataResponse+json",
        "shippingMethods": {
            "LIEFERUNG": "standard_mail",
            "mail": "mail",
            "expressDel": "express_delivery"
        },
        "defaultMethod": "LIEFERUNG"
    }
}
```
