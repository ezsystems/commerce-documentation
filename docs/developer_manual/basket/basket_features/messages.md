#  Messages 

## Basket Messages

Basket messages are stored in the Basket object, but they will not be stored in the database.  It is possible to store several success, error or notice messages for products.

### Methods

<table>
<thead>
<tr class="header">
<th>method</th>
<th>meaning</th>
<th>parameters</th>
</tr>
</thead>
<tbody>
<tr>
<td>setSuccessMessage()</td>
<td>set one success message to the basket</td>
<td>$success</td>
</tr>
<tr>
<td>getSuccessMessages()</td>
<td>get all success messages from the basket. If it is not set an empty string will be returned.</td>
<td><br />
</td>
</tr>
<tr>
<td>setErrorMessage()</td>
<td>set one error message to the basket</td>
<td>$error</td>
</tr>
<tr>
<td>getErrorMessages()</td>
<td>get all error messages from the basket. If it is not set an empty string will be returned.</td>
<td><br />
</td>
</tr>
<tr>
<td>setNoticeMessage()</td>
<td>set one notice message to the basket</td>
<td>$notice</td>
</tr>
<tr>
<td>getNoticeMessages()</td>
<td>get all notice messages from the basket. If it is not set an empty string will be returned.</td>
<td><br />
</td>
</tr>
<tr>
<td>clearAllMessages()</td>
<td>delete all messages from the basket</td>
<td><br />
</td>
</tr>
<tr>
<td>removeSuccessMessageForSku()</td>
<td>deletes all success messages for the given sku from the success messages</td>
<td>$sku</td>
</tr>
</tbody>
</table>

## Messages templating

The messages parsed to the template through the BasketController.

**e.g. basket show**

``` 
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

The messages are shown in the messages.html.twig template

**Silversolutions/Bundle/EshopBundle/Resources/views/Basket/messages.html.twig**

``` 
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
