#  Additional data in the basket line 

## Line remark

There is a possibility to add additional data to the basket line. This data will be stored in a basket line and be send to ERP (in the SesExtension of the line) automatically.

By default you can enable additional text in the basket line:

**basket.yml**

``` 
#enable/disable additional text line in basket per basket line
ses_basket.default.additional_text_for_basket_line: false
```

In the basket line it looks like:

![](attachments/23560228/23571105.png)

To set a different value for the parameter there are 2 possibilities.

  - Override the app/config/parameters.yml file.
  - Within "configuration settings" in eCommerce tab in the backend.

![](attachments/23560228/23571104.png)

The input length of this field is controlled by a setting. This is important since the ERP might not accept text longer than a given limit. 

``` 
ses_basket.default.additional_text_for_basket_line_input_limit: 30
```

## Additional data

To add some additional information to the basket line **only the template** will be modified. No other changes are necessary. See next example.

**Eg. Additional data in the basket line**

``` 
<input type="hidden" name="ses_basket[{{ loop.index }}][test]" value="some text"/>

<input type="text" name="ses_basket[{{ loop.index }}][NewText]" value ="Lorem Ipsum... "/>
 
```

## Attachments:

![](images/icons/bullet_blue.gif) [SettingsBackEnd.png](attachments/23560228/23571104.png) (image/png)  
![](images/icons/bullet_blue.gif) [AdditionalData.png](attachments/23560228/23571105.png) (image/png)  
