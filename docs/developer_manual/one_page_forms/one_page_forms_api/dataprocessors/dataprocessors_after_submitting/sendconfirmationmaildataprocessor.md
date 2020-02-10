# SendConfirmationMailDataProcessor

The goal of this DataProcessor is to send an email for these cases:

  - When a private customer registers, an email to the user email address with an activation link is send.
  - When a business customer registers, an email with all submitted data (including attachment) is send to an admin. The task of admin is to validate the data.
  - When a customer, who has a customer number, was editing his profile data and the data was really changed be him (see [HasFormChangedDataProcessor](HasFormChangedDataProcessor_23560766.html)) an email is send to the administrator.

For sending an email Symfony \\[SwiftMailer](/pages/createpage.action?spaceKey=EZC14&title=Configuration+SwiftMailer&linkCreation=true&fromPageId=23560759) is used.

    ID of this service:
    ses_forms.send_confirmation_data_processor

The mail sender and receiver can be set in the configuration:

**forms.yml**

``` 
siso_core.default.ses_swiftmailer:
    mailSender: noreply@mydomain.de       
    mailReceiver: admin@mydomain.de #used as an admin email address
```

|       |                                                                                                      |
| ----- | ---------------------------------------------------------------------------------------------------- |
| Class | `\Silversolutions\Bundle\EshopBundle\Services\Forms\DataProcessor\SendConfirmationMailDataProcessor` |

## Email Templates

For each FormularType  two template will be rendered. -\> in **.txt** or **.html** format.

**EshopBundle/Resources/views/Emails**

``` 
//Please notice that the name of the template must keep this form: ConfirmationMail_' . $formName . '.txt.twig', ConfirmationMail_' . $formName . '.html.twig'
ConfirmationMail_RegisterBusiness.html.twig
ConfirmationMail_RegisterBusiness.txt.twig

ConfirmationMail_RegisterPrivate.html.twig
ConfirmationMail_RegisterPrivate.txt.twig

ConfirmationMail_MyAccount.html.twig
ConfirmationMail_MyAccount.txt.twig

ConfirmationMail_Buyer.html.twig
ConfirmationMail_Buyer.txt.twig

//Fallback is used when no template was find or defined
ConfirmationMail_Fallback.html.twig
ConfirmationMail_Fallback.txt.twig
```

footer

There is a default mail footer, that is included into emails:

    EshopBundle/Resources/views/Emails/mail_footer.html.twig
