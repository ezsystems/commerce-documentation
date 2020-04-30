# Order splitting for multishop

If there are products from the MAIN shop in the basket, The order is sent to the main shop ERP, and the basket data is modified.

By default this possibility is disabled for the shop, but it is possible to modified this in the configuration: 

`LocalOrderManagementBundle/Resources/config/local_order_management.yml`:

``` yaml
#Enable or disable the possibility to split the order for multishops
siso_local_order_management.default.order_splitting_for_multishop: false
```

For the implementation of this feature event listeners are used.

## Listeners: `onExceptionMessage`

In order to split the orders, the exception "onExceptionMessage" is catch, that will interrupt the default process (sending to ERP).

New listener is created:

`Siso/Bundle/LocalOrderManagementBundle/Resources/config/services.xml`

``` xml
<parameter key="siso_local_order_management.order_splitting.class">Siso\Bundle\LocalOrderManagementBundle\EventListener\OrderSplittingListener</parameter>

<service id="siso_local_order_management.order_splitting" class="%siso_local_order_management.order_splitting.class%">
    <argument type="service" id="ezpublish.config.resolver"/>
    <argument type="service" id="silver_basket.basket_service" />
    <argument type="service" id="silver_basket.basket_repository" />
    <argument type="service" id="siso_tools.mailer_helper" />
    <argument type="service" id="siso_local_order_management.invoice_service" />
    <argument type="service" id="doctrine.orm.entity_manager" />
    <tag name="kernel.event_listener"  event="silver_eshop.exception_message" method="onExceptionMessage" />
</service>
```

Logic inside the new listener:

- Checks if the order contains some products of the MAIN shop.
- Creates a new order only with the products from the "MAIN" shop and data of the shop owner
- Submits the new order to ERP

Some of the products contains a `main_erp` attribute set in the `$remoteDataMap`. To get that information there is a new method inside the `"OrderSplittingListener`

We use a constant called USE_MAIN_ERP_FLAG to define the name for the attribute `main_erp`.

`checkProductsInMainShop`:

``` php
/**
 * Checks the products from the basket.
 * If at least one product of the the basket belongs to main shop, returns true.
 *
 * @param Basket $basket
 * @return bool
 */
protected function checkProductsInMainShop(Basket $basket)
{
    $basketLines = $basket->getLines();
    /** @var BasketLine $basketLine */
    foreach ($basketLines as $basketLine) {
        $remoteDataMap = $basketLine->getRemoteDataMap();

        if (isset($remoteDataMap[self::USE_MAIN_ERP_FLAG]) && !empty($remoteDataMap[self::USE_MAIN_ERP_FLAG])){
            return true;
        }
    }

    return false;
}
```

In order to mark the products from the MAIN shop, a flag can be added in the template.

``` xml
<input type="hidden" name="ses_basket[0][main_erp]" value="1"/>
```

In order to create the new order with products from the main shop there are some conditions, see code below; 

`onExceptionMessage`:

``` php
public function onExceptionMessage(MessageExceptionEvent $messageExceptionEvent)
{
    $exception = $messageExceptionEvent->getException();
    $message = $messageExceptionEvent->getMessage();
    $orderSplittingForMultishop = $this->configResolver->getParameter(
        'order_splitting_for_multishop',
        'siso_local_order_management'
    );

    if ($message instanceof CreateSalesOrderMessage
        && $exception instanceof LocalOrderRequiredException && $orderSplittingForMultishop) {
            
        .....
    
        if ($basket instanceof Basket
            && $this->checkProductsInMainShop($basket)
        ) {
            ...
            $this->webConnectorErpService->submitOrder($copiedBasket);
            .... 
         }
    }
}
```

There is an special attribute that is also set in the DataMap, so it will be possible in the templates to difference if there is a order from the main shop. 

``` php
$copiedBasket->addToDataMap(true, self::MULTISHOP_ORDER);
```

And there is an special method to create the new  basket and the new response:

The basket items are only the ones marked with the main_erp flag.

``` php
/**
 * Copy the current basket and change some information for the splitting order.
 * The copied basket will have the standard logic, send emails, lost orders...
 *
 * @param Basket $basket
 * @return Basket
 *
 */
protected function createMainShopBasket(Basket $basket)
{
    ...

    return $copiedBasket;
}
```

After create the response it sends the confirmation email but without show the price calculation.

``` html+twig
{% if not multishop_order == true %}
    <th style="border-right: 1px solid #999999; border-bottom: 1px solid #999999; font-weight: bold; text-align: center">{{ 'Unit price'|st_translate(null, {}, null, siteaccess) }} {{ '%vat% VAT'|st_translate(null, {'%vat%': vat}, null, siteaccess) }}</th>
    <th style="border-bottom: 1px solid #999999; font-weight: bold; text-align: right">{{ 'Total price'|st_translate(null, {}, null, siteaccess) }}</th>
{% endif %}
```

## Internal Process and implementation

Set the configuration settings (Split orders)

``` 
siso_local_order_management.default.send_order_to_erp: false

siso_local_order_management.default.order_splitting_for_multishop: true
```

### Buyer and invoice party

The buyer party for the split order is generated using the buyer party of shop owner taken from a ERP request.

In Multishop there is a configuration for the customer number of the shop owner.

`shop_owner_customer_number`

This value is the customer number set in ERP.

If there is no ERP connection or there is a temporary problem,

the invoice and buyer address are empty and only filled with the customer number.

``` php
protected function createMainShopBasket(Basket $basket)
{
    $copiedBasket = $this->basketService->copyBasket($basket);
 
    $newParty= new Party();
    $shopOwnerCustomerNumber = $this->configResolver
        ->getParameter('shop_owner_customer_number', 'siso_local_order_management');
 
    //get User information for customer number to fill the address data, because they are needed for the email
    $customerInformation = $this->webConnectorErpService->selectCustomer($shopOwnerCustomerNumber);
    if (isset($customerInformation) && isset($customerInformation->BuyerCustomerParty->Party)){
        $buyerParty = $customerInformation->BuyerCustomerParty->Party;
    } else {
        $buyerParty = new Party();
        $partyIdentification = new PartyPartyIdentification();
        $partyIdentification->ID->value = $shopOwnerCustomerNumber;
        $buyerParty->PartyIdentification[0] = $partyIdentification;
    }
    $copiedBasket->setBuyerParty($buyerParty);
    $copiedBasket->setInvoiceParty($buyerParty);
}
```

### Emails

These emails are send with the standard process in vendor/silversolutions/silver.e-shop/src/Siso/Bundle/CheckoutBundle/EventListener/OrderConfirmationListener.php in the methods onOrderResponse and sendMailToRecipient.

Both emails are based on the standard order confirmation email templates: 

`SilversolutionsEshopBundle:Checkout/Email:order_confirmation.txt.twig` and `SilversolutionsEshopBundle:Checkout/Email:order_confirmation.html.twig`

#### Owner of the shop

Email with the order which was send to ERP
These differences to the standard email are realized by checking the variable 'multishop_order', which is stored in the dataMap of the copied basket:

- a special intro text is set in the email "email_multishop_order_intro_text" (uses  TextModules from the BackEnd
- the delivery and shipping information are hidden
- the basket is displayed without prices
- The image on the top (banner) is different and there is a new block in the template for the image

`{{ block('multishop_email_image') }}`

and it is possible to override it in:

```
{% block multishop_email_image %}
    {% set siteaccess = basket is defined and basket.dataMap.siteaccess is defined ? basket.dataMap.siteaccess : null %}
    <div>
        <img src="{{ absolute_url(asset('bundles/silversolutionseshop/img/email-header.png')) }}"
             alt="{{ 'Please enable loading images to view this image'|st_translate(null, {}, null, siteaccess) }}">
    </div>
{% endblock %}
```

`vendor/silversolutions/silver.e-shop/src/Siso/Bundle/LocalOrderManagementBundle/EventListener/OrderSplittingListener.php`:

``` php
$shopOwnerMailReceiver = isset($emailAddresses['shopOwnerMailReceiver'])
 ? $emailAddresses['shopOwnerMailReceiver'] : '';
$copiedBasket->setConfirmationEmail($shopOwnerMailReceiver);
```

#### Sales Contact person

This email is only sent when the email for the sales contact is configured.

``` php
$recipientSalesContact = $basket->getSalesConfirmationEmail();
if (!empty($recipientSalesContact)) {
    $this->sendMailToRecipient($recipientSalesContact, $basket, true);
}
```

``` yaml
#possible mode: config or customer
siso_checkout.default.order_confirmation.sales_email_mode: customer
siso_checkout.default.order_confirmation.sales_email_address:
```

It is a copy of the email to the owner of the shop with an additional variable is_sales_contact (set to true).
The subject of this email can also differ:

``` php
if (!empty($isSalesContact)){
    $subject = isset($this->mailSettings['sales_contact_subject'])
        ? $this->mailSettings['sales_contact_subject']
        : $subject;
}
```

``` 
 {% if line.catalogElement.sku == '150103' or line.catalogElement.sku == '150106' or line.catalogElement.sku == 'FVA10' %}
         <input type="hidden" name="ses_basket[{{ loop.index }}][MAIN]" value="1"/>
 {% endif %}
```
 
