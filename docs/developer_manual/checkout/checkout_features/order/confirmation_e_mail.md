# Confirmation E-Mail

The shop sends an confirmatuon email to the customer and to a sales contact (if configured).

In case of an failure (e.g. mail server not reachable) the issue will be logged into var/logs/dev/dev-siso.mails.log  (if running in prod mode the file is named with env=prod). 

The logfile contains the recipient, the email text and an error text

## Setting Confirmation Email Address

Processing the confirmation email address is handled in the class:

    \Siso\Bundle\CheckoutBundle\Service\SummaryCheckoutFormService

### Customer confirmation

The customer's confirmation email address depends on the user type and is stored within the basket entity during the checkout process in several steps. There are two options:

  - **ANONYMOUS USER**- email address is taken from the Invoice Address of the basket entity, which is captured during the checkout process.
  - **CUSTOMER WITH OR WITHOUT NUMBER (LOGGED IN)** - email is taken from Customer Profile Data from the eshop

### Sales contact confirmation

Depending on the setup of the e-shop, a second confirmation email is sent to a sales contact. This address is also stored to the basket entity parallel to the customer's address. The behaviour is configurable:

#### siso\_checkout.default.order\_confirmation.sales\_email\_mode

Possible values:

  - `config`
      - The sales contact's email address will be read from the config (the second parameter below)
  - `customer`
      - The sales contact's email address will be read from salesContactEmail from sesUser from the [customer's profile data](Customer-profile-data-model_23560898.html).
      - If no address could be found in the contact data, the configuration parameter below is used as the default.

#### siso\_checkout.default.order\_confirmation.sales\_email\_address

This parameter MAY be set to a valid email address of the sales contact person. If it's left empty, no sales contact confirmations are sent, except if the sales\_email\_mode above is set to "customer" and the respective profile data contains a valid address.

## OrderConfirmationListener

In order to send the order confirmations as soon as an order was successfully accepted by the ERP, a symfony event listener was created:

`\Siso\Bundle\CheckoutBundle\EventListener\OrderConfirmationListener`

This class is configured as a service, which listens to the event `silver_eshop.exception_message`. If the response contains an ERP order number, it is considered as successfull and the confirmation mails are sent as configured. The following parameters are used:

``` 
siso_core.default.ses_swiftmailer:
    mailSender: "mail@example.com"
siso_checkout.default.order_confirmation_subject: "Email subject"
```

## Order confirmation E-Mail

The order confirmation is sent via the `MailHelperService`.  
The confirmation action searches for the basket with the given ID and state "confirmed".

``` 
$basket = $this->getBasket($request, BasketService::STATE_CONFIRMED, $basketId);
```

### Used mail templates

|                 |                                                                                                                                           |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Plain text mail | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/Email/order_confirmation.txt.twig`  |
| HTML mail       | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/Email/order_confirmation.html.twig` |

### Sample screenshot

![](attachments/23560649/23569043.png)

## Attachments:

![](images/icons/bullet_blue.gif) [screenshot\_order\_confirmation.png](attachments/23560649/23563818.png) (image/png)  
![](images/icons/bullet_blue.gif) [email-preview.png](attachments/23560649/23563808.png) (image/png)  
![](images/icons/bullet_blue.gif) [order-confirmation.png](attachments/23560649/23564003.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order Confirmation Mail.pdf.png](attachments/23560649/23569043.png) (image/png)  
