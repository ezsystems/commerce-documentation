# FormProcessorInterface

# Introduction

This interface needs to be implemented by every service that is defined in the *formProcessors* key of a form definition. For example in vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/config/customercenter.yml:

```
    siso_customer_center.default.form.add_user:
        ...
        formProcessors:
            - siso_customer_center.processor.validate_customer_and_contact_number
            ...
```

## FormProcessorInterface

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>public function execute(Form $form, array $lastResult = array());</code></pre></td>
<td><pre><code>Processes the given form data and MUST return the $lastResult parameter with possible changes/additions</code></pre></td>
</tr>
</tbody>
</table>
