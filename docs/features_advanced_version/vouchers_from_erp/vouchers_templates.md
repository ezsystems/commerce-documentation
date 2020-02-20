# Vouchers - Templates

### Templates list

Path: `Silversolutions/Bundle/EshopBundle/Resources/views/Basket/show.html.twig`

Block with vouchers is included here:

``` html+twig
{% block vouchers %}
...
{% endblock %}
```

### Related custom Twig modifiers/functions/etc/:

#### Twig functions

##### ses_contains_basket_vouchers

Returns true if basket contains some vouchers
	
``` html+twig
{% if ses_contains_basket_vouchers(basket) %}
  <a class="button" href="{{ path('siso_remove_voucher') }}">
    <i class="fa fa-times"></i> {{ 'button.remove_vouchers'|st_translate }}
  </a>
{% endif %}
```

##### ses_get_basket_vouchers

Returns list of vouchers from the basket

``` html+twig	
{% set vouchers = ses_get_basket_vouchers(basket) %}
{% for voucher in vouchers %}   
  <p>{{ 'common.voucher_redeemed'|st_translate }} {{ voucher }}</p>
  <a class="button" href="{{ path('siso_remove_voucher', {'voucherNumber' : voucher}) }}">
    <i class="fa fa-times"></i> {{ 'button.remove_voucher'|st_translate }}
  </a>  
{% endfor %}
```

### Related routes:

``` yaml
siso_redeem_voucher:
    pattern: /voucher/redeem
    defaults: { _controller: SisoVoucherBundle:Voucher:redeemVoucher }

siso_remove_voucher:
    pattern: /voucher/remove/{voucherNumber}
    defaults:
        _controller: SisoVoucherBundle:Voucher:removeVoucher
        voucherNumber: null 
```
