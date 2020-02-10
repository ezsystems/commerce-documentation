#  SendContactEmailDataProcessor 

## General

The goal of this data processor is to send an email after the user has filled the contact form. If the user checked that he wants to get a copy of this email, additional email is sent to the user email address also.

By default the email is send to the *contactMailReceiver* who is set in the configuration

**forms.yml**

``` 
parameters:
    siso_core.default.ses_swiftmailer:
          mailSender: noreply@silversolutions.de
          mailReceiver: azh@silversolutions.de
          contactMailReceiver: azh@silversolutions.de
```

    ID of this service:
    siso_core.data_processor.send_contact_email
    
    Corresponding form:
    \Silversolutions\Bundle\EshopBundle\Form\Contact
