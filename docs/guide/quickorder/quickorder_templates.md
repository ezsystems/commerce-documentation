# Quickorder templates

### Templates list

| Path     | Description       |
| -------- | ----------------- |
| `Siso/Bundle/QuickOrderBundle/Resources/views/QuickOrder/quick_order.html.twig`       | Entry page for quickorder   |
| `Siso/Bundle/QuickOrderBundle/Resources/views/QuickOrder/quick_order_form.html.twig` | Renders the content of quickorder page      |
| `Siso/Bundle/QuickOrderBundle/Resources/views/QuickOrder/quick_order_line.html.twig` | Renders the content of one quickorder line. This template is used to replace the quickorder line after the line preview feature. |

### Related routes

``` yaml
siso_quick_order:
    pattern: /quickorder
        defaults: { _controller: SisoQuickOrderBundle:QuickOrder:quickOrder }
```
