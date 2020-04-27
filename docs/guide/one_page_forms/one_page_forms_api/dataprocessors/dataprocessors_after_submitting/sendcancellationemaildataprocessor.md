# SendCancellationEmailDataProcessor

The goal of this data processor is to send an email after the user has filled the cancellation form.

The cancellation mail receiver and the cancellation subject can be set up in the configuration.

``` yaml
parameters:
    #recipient of the cancellation email
    siso_core.default.ses_swiftmailer:
        ...
        cancellationMailReceiver: contact@silversolutions.de

    #cancellation email subject
    siso_core.cancellation.subject: common.cancellation_email_subject
```

ID of this service:

`siso_core.data_processor.send_cancellation_email`
    
Corresponding form:

`\Silversolutions\Bundle\EshopBundle\Form\Cancellation`
