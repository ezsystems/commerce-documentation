#  Vouchers - API 

### Configuration

``` 
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

    Siso/Bundle/VoucherBundle/Service/VoucherManager.php

This service manages general voucher processes, like redeeming or removing of the voucher.

``` 
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
