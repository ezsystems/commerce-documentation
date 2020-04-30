# Basket messages

Basket messages are stored in the Basket object, but they will not be stored in the database.
It is possible to store several success, error or notice messages for products.

### Methods

|method|meaning|parameters|
|--- |--- |--- |
|setSuccessMessage()|set one success message to the basket|$success|
|getSuccessMessages()|get all success messages from the basket. If it is not set, an empty string will be returned.||
|setErrorMessage()|set one error message to the basket|$error|
|getErrorMessages()|get all error messages from the basket. If it is not set, an empty string will be returned.||
|setNoticeMessage()|set one notice message to the basket|$notice|
|getNoticeMessages()|get all notice messages from the basket. If it is not set, an empty string will be returned.||
|clearAllMessages()|delete all messages from the basket||
|removeSuccessMessageForSku()|delete all success messages for the given SKU from the success messages|$sku|

## Messages templating

The messages parsed to the template through the `BasketController`, e.g. basket show:

``` ph
return $this->render(
            'SilversolutionsEshopBundle:Basket:show.html.twig', 
            array(
                'basket' => $basket,
                'error' => $basket->getErrorMessages(),
                'success' => $basket->getSuccessMessages(),
                'notice' => $basket->getNoticeMessages()
            )
); 
```

The messages are shown in the `messages.html.twig` template

`Silversolutions/Bundle/EshopBundle/Resources/views/Basket/messages.html.twig`

``` html+twig
{% if error != '' %}
    {% for e in error %}
        <div class="message error">
            <p><span class="sprite sprite-030f-error">{{ e }}</p>
        
    {% endfor %}
{% endif %}
{% if notice != '' %}
    {% for n in notice %}
        <div class="message notice">
            <p><span class="sprite sprite-030e-notice">{{ n }}</p>
        
    {% endfor %}
{% endif %}
{% if success != '' %}
    {% for s in success %}
        <div class="message success">
            <p><span class="sprite sprite-030g-success">{{ s }}</p>
        
    {% endfor %}
{% endif %}
```
