# SendRmaEmailDataProcessor

The goal of this data processor is to send an email after the user has filled the rma form.

The rma mail receiver and the rma subject can be set up in the configuration.

The rma mail receiver is the same one as the cancellation email receiver

``` yaml
parameters:
    #recipient of the cancellation email
    siso_core.default.ses_swiftmailer:
        ...
        cancellationMailReceiver: contact@silversolutions.de

    #rma email subject
    siso_core.default.rma_subject: "common.rma_email_subject"
```

ID of this service:

`siso_core.data_processor.send_rma_email`

Corresponding form:

`Silversolutions\Bundle\EshopBundle\Form\RMA`
