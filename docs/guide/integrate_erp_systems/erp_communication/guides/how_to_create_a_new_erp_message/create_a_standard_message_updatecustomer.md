# Create a standard message (UpdateCustomer)

## Basics

!!! tip "Basics"

    For a basic knowledge each **message** object in eZ Commerce contains of two parts - *request* and *response* object. We will create them later.

## What does ERP expect?

As a starting point you need to know the request and response XML that is *expected* by ERP.

??? note "Request UPDATECUSTOMER"

    ``` 
    <WC_USER_MANAGEMENT>
        <PROCESS_TYPE>UPDATECUSTOMER</PROCESS_TYPE>
        <GUID>1</GUID>
        <WEB_SITE>HOME</WEB_SITE>
        <NUMBER>1000006</NUMBER>
        <BLOCKED />
        <CURRENCY />
        <WEB_NEWSLETTER>1</WEB_NEWSLETTER>
        <JOB_TITLE />
        <E_MAIL></E_MAIL>
        <NAME>Toni Kreucher</NAME>
        <NAME_2 />
        <ADDRESS />
        <ADDITIONAL_ADDRESS_INFO />
        <CITY>Hamburg</CITY>
        <COUNTRY_CODE />
        <COUNTY />
        <RESPONSIBILITY_CENTER />
        <COMPANY_NUMBER></COMPANY_NUMBER>
        <FAX_NUMBER></FAX_NUMBER>
        <POST_CODE />
        <PHONE_NUMBER></PHONE_NUMBER>
        <HOME_PAGE />
        <E_MAIL />
        <LANGUAGE_CODE>DEU</LANGUAGE_CODE>
        <SALUTATION />
        <SALESPERSON_CODE />
        <CUSTOMER_POSTING_GROUP />
        <GEN_BUS_POSTING_GROUP />
        <VAT_BUS_POSTING_GROUP />
        <CONTACT_PERSON />
        <CREDIT_LIMIT_LCY />
    </WC_USER_MANAGEMENT>
    ```

??? note "Response UPDATECUSTOMER"

    ``` 
    <WC_USER_MANAGEMENT>
        <PROCESS_TYPE>HOME</PROCESS_TYPE>
        <GUID>1</GUID>
        <WEB_SITE>UPDATECUSTOMER</WEB_SITE>
        <NUMBER>1000006</NUMBER>
        <NAME>Toni Kreucher</NAME>
        <CITY>Hamburg</CITY>
        <LANGUAGE_CODE>DEU</LANGUAGE_CODE>
        <CUSTOMER_POSTING_GROUP>INLAND</CUSTOMER_POSTING_GROUP>
        <GEN_BUS_POSTING_GROUP>INLAND</GEN_BUS_POSTING_GROUP>
        <VAT_BUS_POSTING_GROUP>INLAND</VAT_BUS_POSTING_GROUP>
        <WEB_NEWSLETTER>1</WEB_NEWSLETTER>
        <COUNT_OF_SHIP_TO_ADRESSES>0</COUNT_OF_SHIP_TO_ADRESSES>
        <CUST_PRICE_GROUP>
            <CODE>DVGW</CODE>
            <END_DATE>17530101</END_DATE>
            <ASSOCIATION_NO></ASSOCIATION_NO>
        </CUST_PRICE_GROUP>
    </WC_USER_MANAGEMENT>
    ```

!!! tip

    You have to know the webservice operation that will be used for this message.

    If you see that all xml elements are written with capital letters, usually it means that the webservice operation is 'SV\_RAW\_MESSAGE'.

## Define Request and Response Document

!!! tip "Basics"

    eZ Commerce works with PHP objects, not with some XML documents, of course.

    The *request* and *response* document are used as a frame for the generation of PHP classes that will be used in shop. The eZ Commerce generator will use them to create the classes automatically.

With this knowledge we can start with the implementation. Look inside the folder:

``` 
/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml
```

### Message name

First we have to decide how our message will be named.

!!! tip

    It is a good practice to start the name with a small letter and continue with capital letter: **updateCustomer**

Now we have to create two .xml files, one for *request*, one for *response*. Both files will be created in the folder mentioned above.

!!! note "Important naming conventions"

    It is necessary that the names of the .xml files have following structure:

    ```
    request.[messageName].xml
    response.[messageName].xml
    ```
    
    Example:
    
    ```
    vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml/request.updateCustomer.xml
    vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml/response.updateCustomer.xml
    ```

### Request XML

#### Root Tag

create a root tag in your request document:

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml/request.updateCustomer.xml`

!!! tip

    The root tag must be unique in the **target directory** (in our case *vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle*). It is also not a good idea to name it UpdateCustomer (what would be the root tag in the response .xml?)

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<RequestUpdateCustomer>
</RequestUpdateCustomer>
```

#### Content

Now we have to fill it some content. There is a possibility to use some standard .xml documents to become a part of our message. These standard .xml documents are placed in the same directory:

/vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml:

``` 
Contact.xml 
DeliveryParty.xml
LineItem.xml
Party.xml

//You can also create your own structure and define an own document,- it can be used in every message
MyFunnyXml.xml 
```

#### Think about the content structure

If you take a look inside these documents and compare the structure with our UPDATECUSTOMER that is expected by ERP you will find out that there are many commonalities with the Party.xml.

##### UPDATECUSTOMER

``` xml
<WC_USER_MANAGEMENT>
    <PROCESS_TYPE>UPDATECUSTOMER</PROCESS_TYPE>
    <GUID>1</GUID>
    <WEB_SITE>HOME</WEB_SITE>
    <NUMBER>1000006</NUMBER>
    <BLOCKED />
    <CURRENCY />
    <WEB_NEWSLETTER>1</WEB_NEWSLETTER>
    <JOB_TITLE />
    <E_MAIL></E_MAIL>
    <NAME>Toni Kreucher</NAME>
    <NAME_2 />
    <ADDRESS />
    <ADDITIONAL_ADDRESS_INFO />
    <CITY>Hamburg</CITY>
    <COUNTRY_CODE />
    <COUNTY />
    <RESPONSIBILITY_CENTER />
    <COMPANY_NUMBER></COMPANY_NUMBER>
    <FAX_NUMBER></FAX_NUMBER>
    <POST_CODE />
    <PHONE_NUMBER></PHONE_NUMBER>
    <HOME_PAGE />
    <E_MAIL />
    <LANGUAGE_CODE>DEU</LANGUAGE_CODE>
    <SALUTATION />
    <SALESPERSON_CODE />
    <CUSTOMER_POSTING_GROUP />
    <GEN_BUS_POSTING_GROUP />
    <VAT_BUS_POSTING_GROUP />
    <CONTACT_PERSON />
    <CREDIT_LIMIT_LCY />
</WC_USER_MANAGEMENT>
```

##### Party.xml

``` xml
<Party ses_unbounded="PartyIdentification PartyName" ses_type="ses:Contact" ses_tree="SesExtension">
    <PartyIdentification>
       <ID>10000</ID>
    </PartyIdentification>
    <PartyName>
        <Name>Möbel-Meller KG</Name>
    </PartyName>
    <PostalAddress ses_unbounded="AddressLine" ses_tree="SesExtension">
        <StreetName>Tischlerstr. 4-10</StreetName>
        <AdditionalStreetName />
        <BuildingNumber>4-10</BuildingNumber>
        <CityName>Berlin</CityName>
        <PostalZone>12555</PostalZone>
        <CountrySubentity>Berlin</CountrySubentity>
        <CountrySubentityCode>BER</CountrySubentityCode>
        <AddressLine>
            <Line>Gartenhaus</Line>
        </AddressLine>
        <Country>
            <IdentificationCode>DE</IdentificationCode>
            <Name>Deutschland</Name>
        </Country>
        <Department>Development</Department>
        <SesExtension />
    </PostalAddress>
    <Contact>
    </Contact>
    <Person ses_tree="SesExtension">
        <FirstName>Frank</FirstName>
        <FamilyName>Dege</FamilyName>
        <Title>Herr</Title>
        <MiddleName />
        <SesExtension />
    </Person>
    <SesExtension />
</Party>
```

For this reason Party.xml is good enough to be used as a part of our message

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<RequestUpdateCustomer ses_type="ses:Party">
    <Party />
</RequestUpdateCustomer>
```

!!! note "ses_type"

    - Please pay attention if you want to use an external XML document inside your message. In this case `ses_type` is required and the second part after the colon MUST be the refered file name (excluding .xml).
    - The prefix `ses:` will cause the message generator to look up the external XML file within the SilversolutionsEshopBundle and, also, would generate the PHP Objects within SilversolutionsEshopBundle, always. With any other prefix (which SHOULD be `cust:`) the file is looked up in the specified source directory and generated to the specified target directory.

!!! tip

    There are several XML attributes (ses_unbounded, ses_type), that can/shall be used. Please see: [XML format (necessary additional attributes)](../../erp_components/erp_component_messages/erp_message_class_generator.md) to read more.

### Response XML

#### Root Tag

create a root tag in your response document:

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml/response.updateCustomer.xml`

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<ResponseUpdateCustomer ses_type="ses:Party">
</ResponseUpdateCustomer>
```

#### Content

Fill your response document with content

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<ResponseUpdateCustomer ses_type="ses:Party">
    <Party />
</ResponseUpdateCustomer>
```

## Generator

It is completely enough to generate the request and response document for the moment only. Although we might use some *mapping* (we will talk about it later), this is done during the run-time and is not necessary to define it before the PHP classes will be generated.

Now we can use the generator in order to generate the classes automatically.

silversolutions:generatemessages:

``` bash
//Usage
php bin/console silversolutions:generatemessages --message [message name] --sourceDir [path to the request and response .xml dir] --targetDir [path to the target bundle]

//Example
php bin/console silversolutions:generatemessages --message updateCustomer --sourceDir vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml --targetDir vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle

//Re-generation of messages
//If you are re-creating the messages (e.g. the request .xml has been adapted) you have to use to optional --force parameter
php bin/console silversolutions:generatemessages --message updateCustomer --sourceDir vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml --targetDir vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle --force
```

## Mapping

!!! tip "Basics"

    Mapping is done during the run-time and therefore often is enough if only the mapping is overridden in the project - instead of overriding the .xml and re-generation of the messages.

!!! tip "Symlinks"

    ```
    //There should be a symlink to the mapping in app/Resources 
    //xsl - project symlink for the specific project - always has a higher priority
    //xslbase - standard symlink for silver.e-shop as a default
    
    cd app/Resources
    ls -l

    xsl -> ../../src/MyProject/MyProjectBundle/Resources/xsl
    xslbase -> ../../vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase
    ```

Mapping is done inside this folder:

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase`

There are suppose to be two mapping .xsl documents for a message, one for the *request* and one for the *response*.

Important naming conventions

It is *not* necessary that the name of the mapping document stays the same as the message name.

But it is important, that the structure looks like this:

```
request.[mappingName].xsl
response.[mappingName].xsl
```

``` 
//Example
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase/request.updatecustomer.xsl
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase/response.updatecustomer.xsl

//But we could also add something like this:
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase/request.myMapping.xsl
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase/response.myMapping.xsl
```

### Request XSL

As you can see [above](#updatecustomer) - the structure of UPDATECUSTOMER expected by ERP and the Party.xml is not exactly the same. For this reason we have to add mapping- how the information of UPDATECUSTOMER will be mapped into our Party object.

First copy the structure of UPDATECUSTOMER and add it as a content into your request .xsl document.

??? note

    ``` 
    //In the line <xsl:template match="/RequestUpdateCustomer/Party"> you have to add the element of the .xml document which you are going to mapp on - 
    //in our case take a look on our request .xml - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml/request.updateCustomer.xml
    //it is the root tag RequestUpdateCustomer and the Party tag

    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
        <xsl:template match="/RequestUpdateCustomer/Party">
            <WC_USER_MANAGEMENT>
                <PROCESS_TYPE>UPDATECUSTOMER</PROCESS_TYPE>
                <WEB_SITE>HOME</WEB_SITE>
                <NUMBER></NUMBER>
                <BLOCKED />
                <CURRENCY />
                <WEB_NEWSLETTER />
                <JOB_TITLE />
                <NAME></NAME>
                <NAME_2 />
                <ADDRESS></ADDRESS>
                <ADDITIONAL_ADDRESS_INFO></ADDITIONAL_ADDRESS_INFO>
                <CITY></CITY>
                <COUNTRY_CODE></COUNTRY_CODE>
                <COUNTY></COUNTY>
                <RESPONSIBILITY_CENTER />
                <COMPANY_NUMBER />
                <FAX_NUMBER></FAX_NUMBER>
                <POST_CODE></POST_CODE>
                <PHONE_NUMBER></PHONE_NUMBER>
                <HOME_PAGE />
                <E_MAIL></E_MAIL>
                <LANGUAGE_CODE></LANGUAGE_CODE>
                <SALUTATION></SALUTATION>
                <SALESPERSON_CODE />
                <CUSTOMER_POSTING_GROUP></CUSTOMER_POSTING_GROUP>
                <GEN_BUS_POSTING_GROUP></GEN_BUS_POSTING_GROUP>
                <VAT_BUS_POSTING_GROUP></VAT_BUS_POSTING_GROUP>
                <CONTACT_PERSON></CONTACT_PERSON>
                <CREDIT_LIMIT_LCY />
            </WC_USER_MANAGEMENT>
        </xsl:template>
    </xsl:stylesheet>
    ```

Then add mapping information how the attributes of UPDATECUSTOMER are suppose to be mapped into Party attributes.

??? note

    ``` 
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
        <xsl:template match="/RequestUpdateCustomer/Party">
            <WC_USER_MANAGEMENT>
                <PROCESS_TYPE>UPDATECUSTOMER</PROCESS_TYPE>
                <WEB_SITE>HOME</WEB_SITE>
                <NUMBER>
                    <xsl:value-of select="PartyIdentification/ID"/>
                </NUMBER>
                <BLOCKED />
                <CURRENCY />
                <WEB_NEWSLETTER />
                <JOB_TITLE />
                <NAME><xsl:value-of select="PartyName/Name"/></NAME>
                <NAME_2 />
                <ADDRESS><xsl:value-of select="PostalAddress/StreetName"/></ADDRESS>
                <ADDITIONAL_ADDRESS_INFO><xsl:value-of select="PostalAddress/AdditionalStreetName"/></ADDITIONAL_ADDRESS_INFO>
                <CITY><xsl:value-of select="PostalAddress/CityName"/></CITY>
                <COUNTRY_CODE><xsl:value-of select="Country/IdentificationCode"/></COUNTRY_CODE>
                <COUNTY><xsl:value-of select="PostalAddress/CountrySubentity"/></COUNTY>
                <RESPONSIBILITY_CENTER />
                <COMPANY_NUMBER />
                <FAX_NUMBER><xsl:value-of select="Contact/Telefax"/></FAX_NUMBER>
                <POST_CODE><xsl:value-of select="PostalAddress/PostalZone"/></POST_CODE>
                <PHONE_NUMBER><xsl:value-of select="Contact/Telephone"/></PHONE_NUMBER>
                <HOME_PAGE />
                <E_MAIL><xsl:value-of select="Contact/ElectronicMail"/></E_MAIL>
                <LANGUAGE_CODE><xsl:value-of select="Contact/SesExtension/LanguageCode"/></LANGUAGE_CODE>
                <SALUTATION><xsl:value-of select="Person/Title"/></SALUTATION>
                <SALESPERSON_CODE />
                <CUSTOMER_POSTING_GROUP><xsl:value-of select="SesExtension/CustomerPostingGroup"/></CUSTOMER_POSTING_GROUP>
                <GEN_BUS_POSTING_GROUP><xsl:value-of select="SesExtension/GenBusPostingGroup"/></GEN_BUS_POSTING_GROUP>
                <VAT_BUS_POSTING_GROUP><xsl:value-of select="SesExtension/VatBusPostingGroup"/></VAT_BUS_POSTING_GROUP>
                <CONTACT_PERSON><xsl:value-of select="Contact/Name"/></CONTACT_PERSON>
                <CREDIT_LIMIT_LCY />
            </WC_USER_MANAGEMENT>
        </xsl:template>
    </xsl:stylesheet>
    ```

### Response XSL

We have to add a backward mapping from Party attributes into UPDATECUSTOMER attributes. For this reason copy the content of the Party.xml and use it as a content for your response .xsl. Then add mapping informations how the attributes of Party are suppose to be mapped into UPDATECUSTOMER attributes.

!!! note

    It may be possible that some standard .xml document such as Party.xml contains other standard documents such as Contact.xml. If this occurs you also have to copy the content of the sub-documents.

??? note

    ``` 
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
        <xsl:template match="/*">
            <ResponseUpdateCustomer>
                <Party>
                    <PartyIdentification>
                        <ID>
                            <xsl:value-of select="NUMBER"/>
                        </ID>
                    </PartyIdentification>
                    <PartyName>
                        <Name><xsl:value-of select="NAME"/></Name>
                    </PartyName>
                    <PostalAddress ses_unbounded="AddressLine" ses_tree="SesExtension">
                        <StreetName><xsl:value-of select="ADDRESS"/></StreetName>
                        <AdditionalStreetName><xsl:value-of select="ADDITIONAL_ADDRESS_INFO"/></AdditionalStreetName>
                        <BuildingNumber />
                        <CityName><xsl:value-of select="CITY"/></CityName>
                        <PostalZone><xsl:value-of select="POST_CODE"/></PostalZone>
                        <CountrySubentity><xsl:value-of select="COUNTY"/></CountrySubentity>
                        <AddressLine>
                            <Line />
                        </AddressLine>
                        <Country>
                            <IdentificationCode><xsl:value-of select="COUNTRY_CODE"/></IdentificationCode>
                        </Country>
                        <SesExtension />
                    </PostalAddress>
                    <Contact>
                        <Name><xsl:value-of select="CONTACT_PERSON"/></Name>
                        <Telephone><xsl:value-of select="PHONE_NUMBER"/></Telephone>
                        <Telefax><xsl:value-of select="FAX_NUMBER"/></Telefax>
                        <ElectronicMail><xsl:value-of select="E_MAIL"/></ElectronicMail>
                        <SesExtension>
                            <LanguageCode><xsl:value-of select="LANGUAGE_CODE"/></LanguageCode>
                        </SesExtension>
                    </Contact>
                    <Person ses_tree="SesExtension">
                        <Title><xsl:value-of select="SALUTATION"/></Title>
                    </Person>
                    <SesExtension>
                        <CustomerPostingGroup><xsl:value-of select="CUSTOMER_POSTING_GROUP"/></CustomerPostingGroup>
                        <GenBusPostingGroup><xsl:value-of select="GEN_BUS_POSTING_GROUP"/></GenBusPostingGroup>
                        <VatBusPostingGroup><xsl:value-of select="VAT_BUS_POSTING_GROUP"/></VatBusPostingGroup>
                    </SesExtension>
                </Party>
            </ResponseUpdateCustomer>
        </xsl:template>
    </xsl:stylesheet>
    ```

## Message configuration

We also have to configure the message. Our [generator](#generator) has created some frames for us already.

By default the messages are configured in the messages.yml.

You need to add imports and parameters!

vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/messages.yml:

``` 
//Import the auto-generated listener here
imports:
    - { resource: "@SilversolutionsEshopBundle/Resources/config/updatecustomerfactorylistener.service.yml" }

//Add message configuration
parameters:
    silver_erp.config.messages:
        ...
        //take a look in the auto-generated vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/updatecustomermessage.message.yml
        updatecustomer:
            //path to the auto-generated message
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\UpdateCustomerMessage"
            //path to the auto-generated response class
            response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\ResponseUpdateCustomer"
            //name of the webservice operation
            webservice_operation: "SV_RAW_MESSAGE"
            //mapping name
            mapping_identifier: "updatecustomer"
```
