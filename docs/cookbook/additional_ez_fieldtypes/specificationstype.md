# SpecificationsType

The FieldType is used to store a structured list of attributes for products:

![](../img/additional_ez_fieldtypes_10.png)

The data is stored in json format.

``` json
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

![](../img/additional_ez_fieldtypes_11.png)

``` yaml
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

``` yaml
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

`SilversolutionsDatatypesBundle::sesselectiontype_content_field.html.twig`

## Using SpecificationsType in a content field definition for a content type

!!! note "Field identifier naming restriction"

    The SpecificationsType requires that its field identifier is set to `ses_specifications`
