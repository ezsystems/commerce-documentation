# One-page forms FAQ

## Can I change the min length for the phone validator?

Yes, you can configure the min and max length for the [phone validator](one_page_forms_api/one_page_forms_api.md).

``` yaml
ses_phone_validator:
    min: 9
    max: 15
```

## Which forms are prepared for reCAPTCHA?

- Contact form
- Cancellation form
- Registration forms
  - private
  - business
- Activate business

## Where can the reCAPTCHA be activated for forms?

The reCAPTCHA can be activated in the Configuration Settings of eZ Commerce.

## What can be configured at the moment?

It is possible to configure the following options in the "Configuration settings":

- theme (light or dark)
- type (audio or image)
- size (normal or compact)
- defer (When present, it specifies that the script is executed when the page has finished parsing.)
- async (When present, it specifies that the script will be executed asynchronously as soon as it is available.)

## reCaptcha confirms that I'm not a robot without assigning a task, why?

Google's risk analysis is sure that you're human, in this case you won't be bothered with a reCaptcha task.
