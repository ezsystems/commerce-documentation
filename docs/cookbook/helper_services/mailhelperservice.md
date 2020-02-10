#  MailHelperService 

The `MailHelperService `is used to create, render and send various kind of e-mails for eZ Commerce.

## Interface and implementations

|                                           |                                                                                                          |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Interface definition                      | `vendor/silversolutions/silver.tools/src/Siso/Bundle/ToolsBundle/Service/MailHelperServiceInterface.php` |
| Implementation class (using Swift-Mailer) | `vendor/silversolutions/silver.tools/src/Siso/Bundle/ToolsBundle/Service/SwiftMailHelperService.php`     |

## Using

Please note that the $subject is translated in the helper service. Translating it in advance is unnecessary or, in rare cases, could even cause conflicts.

The `SwiftMailHelperService` uses the [Mail Logging](Mail-Logging_23560245.html) in order to provide administrators to keep track of all sent e-mails. PLEASE NOTE: There is a special case for the [Customer Center (from 4.2)](/pages/createpage.action?spaceKey=EZC14&title=Customer+Center+%28from+4.2%29&linkCreation=true&fromPageId=23560658). The content of any template parameter, which is named 'password', will be logged with a masked/removed value. This was needed for notification e-mails for new customer center accounts, which contain a generated, temporary password.

You are able to send predefined plain-text and/or HTML content by using sendMail(), or use the convinient sendMailWithRenderedTemplate() method.

``` 
$sender = 'fd@silversolutions.de';
$recipient = 'rel@silversolutions.de';
$subject = 'Test Mail';
/** @var \Siso\Bundle\ToolsBundle\Service\MailHelperServiceInterface $mailerService */
$mailerService = $container->get('siso_tools.mailer_helper');
$attachment = '/path/to/file/to/attach.jpg'; // optional
 
// using sendMail()
$mailerService->sendMail(
    $sender,
    $recipient,
    $subject,
    'This is my plain text mail',
    '<html><body><h1>HTML Mail</h1>My HTML mail</body></html>',
    $attachment
);
 
// using sendMailWithRenderedTemplate() with Twig template paths
$mailerService->sendMailWithRenderedTemplate(
    $sender,
    $recipient,
    $subject,
    'SilversolutionsEshopBundle:Checkout/Email:order_confirmation.txt.twig',    // template for plain-text mail
    'SilversolutionsEshopBundle:Checkout/Email:order_confirmation.html.twig',   // template for HTML mail
    array(                                                                      // optional Twig parameters
        'template_param_1' => array('test', 'array'),
        'template_param_2' => 'this is a parameter for the templates',
    ),
    $attachment
);
```
