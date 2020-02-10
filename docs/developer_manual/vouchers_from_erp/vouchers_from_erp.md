#  Vouchers from ERP 

## Introduction

Vouchers in eZ Commerce can be managed in:

  - ERP
  - Shop Admin Interface - CMS

eZ Commerce supports vouchers that are managed in ERP. The user knows the voucher number and can enter it in the basket. Then the voucher is send to the ERP and if valid, user gets a discount.

## Important terms used in this document

ERP

**ERP** (Enterprise resource planning) is business [management](http://en.wikipedia.org/wiki/Management "Management") software—typically a suite of integrated applications—that a company can use to collect, store, manage and interpret data from many business activities, including:

  - [Product planning](http://en.wikipedia.org/wiki/Product_planning "Product planning"), cost
  - [Manufacturing](http://en.wikipedia.org/wiki/Manufacturing "Manufacturing") or service delivery
  - [Marketing](http://en.wikipedia.org/wiki/Marketing "Marketing") and sales
  - [Inventory management](http://en.wikipedia.org/wiki/Inventory_management "Inventory management")
  - Shipping and payment

*Source*: Wikipedia

Known ERP software packages:

  - <span class="internal">Microsoft Dynamics NAV (former navision)
  - <span class="internal">Microsoft Dynamics AX  
    
  - SAP

CMS

**CMS**  (content management system) is a software for the collective creation, editing and organization of content   mostly in web pages, but also in other forms of media. This can consist  text and multimedia documents. An author with access rights can such a system, in many cases with little programming or HTML knowledge use, since the majority of systems have a graphical user interface.

*Source*: Wikipedia  

## Before you start 

Please keep in mind that Vouchers is really connected with a lot of different modules in your shop. Be sure to check these out:

  - [ERP](ERP-communication_23560973.html)
  - [Basket](Basket_23560477.html)
  - [PriceEngine](Price-Engine_23560375.html)
  - [Checkout](Checkout_23560414.html)

## FAQ

How the voucher data is send to ERP?

Voucher data will be sent to ERP in the **PriceRequest** and **CreateOrderRequest**.

If enabled in the [configuration](https://doc.silver-eshop.de/display/EZC14/Vouchers+-+API), also additional line with negative quantity will be sent. NAV must response with negative cost.

#### Data send in the header

``` 
//Data sent in the header, in SesExtension:
<SesExtension>
    <voucherNumber>[voucher_number]</voucherNumber> 
    <voucherNumber>[voucher_number2]</voucherNumber>  
</SesExtension>

//Additional line sent
<OrderLine>    
    <LineItem>
        <Item>
            <SellersItemIdentification>
                <ID>[voucher_number]</ID>
            </SellersItemIdentification>
        </Item>
        <Quantity>-1</Quantity>
        <Price>
            <PriceAmount></PriceAmount>
        </Price>
    </LineItem>
    <SesExtension>
        <isVoucher>1</isVoucher>
        <voucherNumber>[voucher_number]</voucherNumber>        
    </SesExtension>
</OrderLine>
```

What happenes if voucher is invalid?

ERP can send a message that the voucher is invalid. In that case this message will be displayed in the basket. The message must be mapped into Response like this:

``` 
<SesExtension>
    <ErpMessage>008</ErpMessage>
</SesExtension>
```

In addition in the OrderLine (Response) the LineType and CostType has to be set:

``` 

<SesExtension>
    <ErpMessage>008</ErpMessage>
</SesExtension>
```

Does the price engine has access to a voucher code?

The priceRequest contains the voucher code:

![](attachments/23560703/23563263.png)

**Important**:

A use can provide one or more vouchers during checkout. The extendedData structure provides simple data structures only. This is why the field name contains the voucher number as well (here voucher\_200).

## Templating

The Vouchers is using a list of templates which can be overridden if required. 

[Read more...](Vouchers---Templates_23560725.html)

## Cookbook

Find recipes in [cookbook...](Vouchers---Cookbook_23560700.html)

## API

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
