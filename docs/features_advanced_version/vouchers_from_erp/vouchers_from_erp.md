# Vouchers from ERP

## Introduction

Vouchers in eZ Commerce can be managed in:

- ERP
- Shop Admin Interface - CMS

eZ Commerce supports vouchers that are managed in ERP. The user knows the voucher number and can enter it in the basket. Then the voucher is send to the ERP and if valid, user gets a discount.

## Before you start 

Please keep in mind that Vouchers is really connected with a lot of different modules in your shop. Be sure to check these out:

- [ERP](../integrate_erp_systems/erp_communication/erp_communication.md)
- [Basket](../../../developer_manual/basket/basket.md)
- [PriceEngine](../../../developer_manual/price_engine/price_engine.md)
- [Checkout](../../../developer_manual/checkout/checkout.md)

## Templating

The Vouchers is using a list of templates which can be overridden if required. 

[Read more...](vouchers_templates.md)

## API

### Configuration

``` yaml
twig:
  globals:
    # if false vouchers should not be active in the project
    # - then user can not enter the voucher number in the basket
    voucher_active: true

parameters:
    # if true the vocher will be send to ERP as additional line with negative quantity
    # otherwise it will be send just in the header
    siso_voucher.default.send_vouchers_as_lines: true
```

### VoucherManager

`Siso/Bundle/VoucherBundle/Service/VoucherManager.php`

This service manages general voucher processes, like redeeming or removing of the voucher.

``` php
$voucherNumber = '123456789';

$basketService = $this->getBasketService();
$basket = $basketService->getBasket($request);

//get the voucher manager
$voucherManager = $this->get('siso_voucher.voucher_manager'); 

//redeem the voucher
$voucherManager->redeemVoucherNumber($basket, $voucherNumber);

//remove the voucher
$voucherManager->removeVoucher($basket, $voucherNumber);
```
