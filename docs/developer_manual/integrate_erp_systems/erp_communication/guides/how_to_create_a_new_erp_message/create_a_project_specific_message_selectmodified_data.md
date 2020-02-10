#  Create a project specific message (SelectModifiedData) 

## Basics

Basics

For a basic knowledge each **message** object in eZ Commerce contains of two parts -  *request* and *response* object. We will create them later.

## What does ERP expect?

As a starting point you need to know the request and response XML that is *expected* by ERP.

**Request SELECTMODIFIEDDATA**

``` 
<WC_MANAGEMENT>
    <PROCESS_TYPE>GETNEWCONTACT</PROCESS_TYPE>
    <GUID>1</GUID>
    <WEB_SITE>HOME</WEB_SITE>
    <ENTRY_NO>43</ENTRY_NO>
    <MAXCOUNT>10</MAXCOUNT>
</WC_MANAGEMENT>
```

**Response SELECTMODIFIEDDATA**

``` 
<WC_MANAGEMENT>
    <PROCESS_TYPE>GETNEWCONTACT</PROCESS_TYPE>
    <GUID />
    <WEB_SITE>HOME</WEB_SITE>
    <ENTRY_NO>43</ENTRY_NO>
    <MAXCOUNT>10</MAXCOUNT>
    <CUSTOMER>
        <LINE>
            <CUSTOMER_NO>POITOU03</CUSTOMER_NO>
            <CONTACT_NO>CON0358</CONTACT_NO>
            <ENTRY_NO>43</ENTRY_NO>
        </LINE>
    </CUSTOMER>
    <LAST_ENTRY_NO>43</LAST_ENTRY_NO>
</WC_MANAGEMENT>
```

You have to know the webservice operation that will be used for this message.

If you see that all xml elements are written with capital letters, usually it means that the webservice operation is 'SV\_RAW\_MESSAGE'.

## Define Request and Response Document

Basics

eZ Commerce works with PHP objects, not with some XML documents, of course.

The *request* and *response* document are used as a frame for the generation of PHP classes that will be used in shop. The eZ Commerce generator will use them to create the classes automatically.

With this knowledge we can start with the implementation. Look inside this folder. (If it´s not there, create it). This is the place where we will begin our implementation.

``` 
/src/MyCompany/Bundle/MyCompanyBundle/Resources/specifications/xml
```

The folder mentioned above is following the structure convetions.

*/src/MyCompany/Bundle/MyCompanyBundle/Resources/specifications/xml*  
  
You can decide to use different structrure, if you want  
Example:

/*src/MyCompany/Bundle/MyCompanyBundle/Resources/xml*  
  
The path to this folder will be set up anyway when you will use command to generate the messages.

### Message name

First we have to decide how our message will be named.

It is a good practice to start the name with a small letter and continue with capital letter: **selectModifiedData**

Now we have to create two .xml files, one for *request*, one for *response*. Both files will be created in the folder mentioned above.

Important naming conventions

    It is necessary that the names of the .xml files have following structure:

    request.[messageName].xml
    response.[messageName].xml
    
    Example:
    src/MyCompany/Bundle/MyCompanyBundle/Resources/specifications/xml/request.selectModifiedData.xml
    src/MyCompany/Bundle/MyCompanyBundle/Resources/specifications/xml/response.selectModifiedData.xml

### Request XML

just copy the content of the request SELECTMODIFIEDDATA and use it as a content four your request .xml:

    src/MyCompany/Bundle/MyCompanyBundle/Resources/specifications/xml/request.selectModifiedData.xml

``` 
<?xml version="1.0" encoding="UTF-8"?>
<WC_MANAGEMENT>
    <PROCESS_TYPE>GETNEWCONTACT</PROCESS_TYPE>
    <GUID>1</GUID>
    <WEB_SITE>HOME</WEB_SITE>
    <ENTRY_NO>43</ENTRY_NO>
    <MAXCOUNT>10</MAXCOUNT>
</WC_MANAGEMENT>
```

The root tag must be unique in the **target directory** (in our case *vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle*).

### Response XML

just copy the content of the response SELECTMODIFIEDDATA and use it as a content four your response .xml:

    src/MyCompany/Bundle/MyCompanyBundle/Resources/specifications/xml/response.selectModifiedData.xml

``` 
<?xml version="1.0" encoding="UTF-8"?>
<WC_USER_MANAGEMENT ses_unbounded="LINE">
    <PROCESS_TYPE />
    <GUID />
    <WEB_SITE />
    <ENTRY_NO />
    <MAXCOUNT />
    <CUSTOMER>
        <LINE>
            <CUSTOMER_NO />
            <CONTACT_NO />
            <ENTRY_NO />
        </LINE>
    </CUSTOMER>
    <LAST_ENTRY_NO />
</WC_USER_MANAGEMENT>
```

There are several XML attributes (ses\_unbounded, ses\_type), that can/shall be used. Please see: [XML format (necessary additional attributes)](ERP-Message-Class-Generator_23560376.html) to read more.

## Generator

It is completely enough to generate the request and response document only. 

Now we can use the generator in order to generate the classes automatically.

<span id="Createaprojectspecificmessage(SelectModifiedData)-Generator" class="confluence-anchor-link">

**silversolutions:generatemessages**

``` 
//Usage
php bin/console silversolutions:generatemessages --message [message name] --sourceDir [path to the request and response .xml dir] --targetDir [path to the target bundle]

//Example
php bin/console silversolutions:generatemessages --message selectModifiedData --sourceDir src/MyCompany/Bundle/MyCompanyBundle/Resources/specifications/xml --targetDir src/MyCompany/Bundle/MyCompanyBundle

//Re-generation of messages
//If you are re-creating the messages (e.g. the request .xml has been adapted) you have to use to optional --force parameter
php bin/console silversolutions:generatemessages --message selectModifiedData --sourceDir src/MyCompany/Bundle/MyCompanyBundle/Resources/specifications/xml --targetDir src/MyCompany/Bundle/MyCompanyBundle --force
```

## Mapping

Symlinks

    //There should be a symlink to the mapping in app/Resources 
    //xsl - project symlink for the specific project - always has a higher priority
    //xslbase - standard symlink for silver.e-shop as a default
    
    cd app/Resources
    ls -l

    xsl -> ../../src/MyCompany/Bundle/MyCompanyBundle/Resources/xsl
    xslbase -> ../../vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase

Mapping is done inside this folder:

``` 
src/MyCompany/Bundle/MyCompanyBundle/Resources/xsl
```

If you are creating a new message that will be specific only for the project and you are not using any standard .xml documents as a part of your message, usually you don´t need to define any kind of mapping.

## Message configuration

We also have to configure the message. Our [generator](#Createaprojectspecificmessage\(SelectModifiedData\)-Generator) has created some frames for us already.

You can create a special .yml file only for the messages configuration, such as **project**.**messages.yml**

**src/MyCompany/Bundle/MyCompanyBundle/Resources/config/project.messages.yml**

``` 
//Import the auto-generated listener here
imports:
    - { resource: "@MyCompanyBundle/Resources/config/selectmodifieddatafactorylistener.service.yml" }

//Add message configuration
parameters:
    silver_erp.config.messages:
        ...
        //take a look in the auto-generated src/MyCompany/Bundle/MyCompanyBundle/Resources/config/selectmodifieddatamessage.message.yml
        selectmodifieddata:
            //path to the auto-generated message
            message_class: "\\MyCompany\\Bundle\\MyCompanyBundle\\Entities\\Messages\\SelectModifiedDataMessage"
            //path to the auto-generated response class
            response_document_class: "\\MyCompany\\Bundle\\MyCompanyBundle\\Entities\\Messages\\Document\\SelectModifiedDataResponse"
            //name of the webservice operation
            webservice_operation: "SV_RAW_MESSAGE"
            //mapping name - if you don´t have any mapping, just leave it empty
            mapping_identifier: ""
```
