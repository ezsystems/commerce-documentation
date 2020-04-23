# Price

Namespace: `Siso\Bundle\PriceBundle\Model`

The `Price` class defines a generic price object. When attached to a [`ProductNode`](../../../catalog/catalog_api/productnode_and_orderableproductnode.md) object, the Field [`PriceField`](../../../../cookbook/fields_for_ecommerce_data/pricefield.md) is used.

As this class is extending [`ValueObject`](../../../../cookbook/valueobject.md), you can set the necessary properties a new `Price` object easily using an array in the constructor.

## Properties

| Name               | Type      | Mandatory | Description                |
| ------------------ | --------- | :-------: | ------------------------------------------------------------------ |
| `price`            | `float`   |    yes    | The main price value                                               |
| `priceInclVat`     | `float`   |    no     | The price value definitive including VAT                           |
| `priceExclVat`     | `float`   |    no     | The price value definitive excluding VAT                           |
| `isVatPrice`       | `boolean` |    yes    | If true, the main price value is including VAT                     |
| `vatCode`          | `float`   |    yes    | The VAT percentage (might be Field in future)                      |
| `currency`         | `string`  |    yes    | The ISO 4217 currency code (e.g. "EUR", "USD", "GBP", ...)         |
| `discount`         | `float`   |    no     | Discount in percentage                                             |
| `timestamp`        | `string`  |    no     | Timestamp information                                              |
| `campaign`         | `string`  |    no     | Campaign code resp. value                                          |
| `source`           | `string`  |    yes    | The source of this price (e.g. "NAV")                              |
| `quantityDiscount` | `array`   |    no     | Information regarding block prices etc. (might be Field in future) |
