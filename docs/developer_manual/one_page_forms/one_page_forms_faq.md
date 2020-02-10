#  One-page forms - FAQ 

Can I change the min length for the phone validator?

 Yes, you can configure the min and max length for the [phone validator](One-page-forms---API_23560842.html).

``` 
ses_phone_validator:
    min: 9
    max: 15
```

Which forms are prepared for reCAPTCHA?

  - Contact form
  - Cancellation form
  - Registration forms
      - private
      - business
  - Activate business

Where can the reCAPTCHA be activated for forms?

The reCAPTCHA can be activated in the Configuration Settings of eZ Commerce.

 ![](plugins/servlet/confluence/placeholder/unknown-attachment "image2017-1-18 9:31:40.png")

What can be configured at the moment?

It is possbile to configure the following options in the "Configuration settings":

  - theme (light or dark)
  - type (audio or image)
  - size (normal or compact)
  - defer (When present, it specifies that the script is executed when the page has finished parsing.)
  - async (When present, it specifies that the script will be executed asynchronously as soon as it is available.)

reCaptcha confirms that I'm not a robot without assigning a task, why?

Google’s risk analysis is sure that you’re human, in this case you won't be bothered with a reCaptcha task.

Quote from Google

The new reCAPTCHA is here. A significant number of your users can now attest they are human without having to solve a CAPTCHA. Instead with just a single click they’ll confirm they are not a robot. We’re calling it the No CAPTCHA reCAPTCHA experience.
