#  SesProfileData 

This datatype is used to store the [CustomerProfileData](Customer-profile-data-model_23560898.html) in the User class in eZ Platform.

CustomerProfileData must be stored in eZ as a serialized string in **base64** format, since it is not possible to store any special HTML characters(\<,\>, "",'', &) in textfield or textarea field.

# Symfony Datatype

Symfony Datatype is stored in:

    Silversolutions/Bundle/DatatypesBundle/FieldType/SesProfileData/*

    Silversolutions/Bundle/DatatypesBundle/Converter/SesProfileData.php

#### Configuration

**Silversolutions/Bundle/DatatypesBundle/Resources/config/services.xml**

``` 
    <parameters>
        <parameter key="ezpublish.fieldType.sesprofiledata.class">Silversolutions\Bundle\DatatypesBundle\FieldType\SesProfileData\Type</parameter>
        <parameter key="ezpublish.fieldType.sesprofiledata.converter.class">Silversolutions\Bundle\DatatypesBundle\Converter\SesProfileData</parameter>
    </parameters>

    <services>      
        <!-- sesprofiledata type service -->
        <service id="ezpublish.fieldType.sesprofiledata" class="%ezpublish.fieldType.sesprofiledata.class%" parent="ezpublish.fieldType">
            <tag name="ezpublish.fieldType" alias="sesprofiledata" />
        </service>

        <!-- sesprofiledata converter service -->
        <service id="ezpublish.fieldType.sesprofiledata.converter" class="%ezpublish.fieldType.sesprofiledata.converter.class%">
            <tag name="ezpublish.storageEngine.legacy.converter" alias="sesprofiledata"  />
        </service>      
    </services> 
```

The name of the customer (used from the contact section) can be used in the backend for lists. This can be achieved by using the name pattern in the class definition of the ezuser class:

![](attachments/23560919/23563984.png)

"customer\_profile\_data" is the identifier of the attribute where the profile data is stored.

## Attachments:

![](images/icons/bullet_blue.gif) [User\_\_\_Users\_\_\_Class\_groups\_-\_eZ\_Publish\_Demo\_Site\_\_without\_demo\_content\_.png](attachments/23560919/23563984.png) (image/png)  
