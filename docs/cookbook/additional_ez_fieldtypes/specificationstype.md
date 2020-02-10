#  SpecificationsType 

The FieldType is used to store a structured list of attributes for products:

![](attachments/23560733/23562928.png)

The data is stored in json format.

``` 
[
  {
    "name": "marketing",
    "data": [
      {
        "label": "Brand",
        "value": "MG"
      },
      {
        "label": "Warranty",
        "value": "2yrs"
      }
    ]
  },
  {
    "name": "technic",
    "data": [
      {
        "label": "Size",
        "value": "12cm"
      },
      {
        "label": "color",
        "value": "red"
      }
    ]
  }
]
```

## Adding a unit selection

The optional attribute "option" allows to add a select field offering e.g. a selection of units:

![](attachments/23560733/23570823.png)

``` 
-
    code: "technic"
    label: "Technical data"
    default_values:
        -
            label: "Width"
            value: ""
            options: ['mm','cm','m']
        -
            label: "Heigth"
            value: ""
            options: ['mm','cm','m']
        -
            label: "Length"
            value: ""
            options: ['mm','cm','m']
        -
            label: "Weight"
            value: ""
            options: ['g','kg']
```

## Adding default values

The FieldType is using a configuration which controls which groups will be offered. Default attributes can be provided as well:

``` 
siso_core.default.specification_groups:
    -
        code: "marketing"
        label: "Marketing data"
        default_values:
            -
                label: "Brand"
                value: ""
    -
        code: "technic"
        label: "Technical data"
        default_values: ~
    -
        code: "norms"
        label: "Standards"
        default_values: ~
```

## Used template:

SilversolutionsDatatypesBundle::sesselectiontype\_content\_field.html.twig

## Using SpecificationsType in a content field definition for a content type

Field identifier naming restriction

The SpecificationsType requires that its field identifier is set to "ses\_specifications"

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-7-17\_18-57-58.png](attachments/23560733/23571062.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-11-9\_14-58-23.png](attachments/23560733/23562928.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2019-3-28\_19-38-45.png](attachments/23560733/23570823.png) (image/png)  
