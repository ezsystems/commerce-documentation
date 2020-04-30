# REST API (Version 4.2)

eZ Commerce extends the Rest API of eZ Platform with:

## Basket API

| Feature | Method | Notes |
| ------- | ------ | ----- |
|Get current basket	|/api/ezp/v2/siso-rest/basket/current||
|Reads list of baskets|	/api/ezp/v2/siso-rest/basket/header/{basket_type}|Returns wishlist/stored baskets|
|Updates party information in the basket|/api/ezp/v2/siso-rest/basket/current/party/invoice (PATCH)</br>/api/ezp/v2/siso-rest/basket/current/party/delivery (PATCH)</br>/api/ezp/v2/siso-rest/basket/current/party/buyer (PATCH)||
|Update shipping infos|	/api/ezp/v2/siso-rest/basket/current/shippingmethod ||
|Update payment infos|	/api/ezp/v2/siso-rest/basket/current/paymentmethod	||
|Update and check voucher|	/api/ezp/v2/siso-rest/basket/current/voucher||
|Adds a product to a basket|	/api/ezp/v2/siso-rest/basket/current/lines||
|Update infos in the basketline	|/api/ezp/v2/siso-rest/basket/update ||

## Checkout API

| Feature | Method | Notes |
| ------- | ------ | ----- |
|Get list of payment methods | /api/ezp/v2/siso-rest/checkout/payment-methods	||
|get list of shipping methods | /api/ezp/v2/siso-rest/checkout/shipping-methods	||

## Common API

| Feature | Method | Notes |
| ------- | ------ | ----- |
|Get list of payment methods | /api/ezp/v2/siso-rest/country-selection ||

## Customer API

The eZ API already offers a lot of features to add, modify or remove user

| Feature | Method | Notes |
| ------- | ------ | ----- |
|Get list of shipping addresses from customer | /api/ezp/v2/siso-rest/customer/addresses/shipping||
