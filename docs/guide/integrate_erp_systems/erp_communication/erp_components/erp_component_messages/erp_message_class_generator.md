# ERP Message-Class-Generator

## XML to PHP generator

The XML to PHP generator is used to create Plain Old PHP Objects (POPO) out of sample XML files.

**This component will:**

- Read a pair of custom XML documents of a specified ERP message (request and response)
- Generate POPOs with a constructor which will initialize all members
- Generate a message class which contains a reference to the request and response POPO
- Generate a factory class for the generated message class
- Generate all necessary configuration files which may be imported or copied into the projects configuration

**Terms:**

- **Message**: A unit of a request and response document
- **Document**: A single XML document for ERP communication. One document may be sent to the ERP and another document may be received from the ERP
**Simple document example**

``` 
<?xml version="1.0" encoding="UTF-8"?>
<PartyRequest>
    <Party>
        <Id>1234</Id>
        <Name>Example & Co.</Name>
        <Address>
            <FirstName>Peter</FirstName>
            <LastName>Smith</LastName>
            <Street>Long Street 12</Street>
            <PostalCode>12345</PostalCode>
            <City>Big City</City>
            <Region>Random County</Region>
        </Address>
    </Party>
</PartyRequest>
```

- **POPO**: Plain Old PHP Object (a PHP object without any / further logic in its methods).
    - For a concept from Java see <http://www.javaleaks.org/open-source/php/plain-old-php-object.html> )

## Usage

### XML format (necessary additional attributes)

For the automated generation of PHP classes, additional information in the XML are necessary. The XML can be extended with the following attributes:

#### ses_type

Required for elements, which are defined in external XML files and provide standard elements.

If an element value is a complex type which is intended to be reused in several messages (e.g. a business party record) it must be defined in this attribute.
The value represents a space separated list of child element names that are supposed to be of the reusable type. The name of the elements will also be the name of the applicable (POPO-)classes.

The names of the types SHOULD be prefix with either ses: or cust:

- ses: type classes would be stored into the eZ Commerce path. XML definition files are always read from the eZ Commerce path.
- cust: type classes would be stored into the defined target directory. XML definition files are always read from the source path.

For each type an XML file MUST be stored in the src folder which defines the reusable datatype which is named <elementName>.xml

``` xml
Example (request.selectContact.xml): 
<?xml version="1.0" encoding="UTF-8"?>
<BuyerCustomerParty ses_type="ses:Party">
    <Party>
    </Party>
</BuyerCustomerParty>
```

Example (Party.xml):

``` xml
<Party ses_unbounded="Address">
    <Id></Id>
    <Name></Name>
    <Address>
        <FirstName></FirstName>
        <LastName></LastName>
        <Street></Street>
        <PostalCode></PostalCode>
        <City></City>
        <Region></Region>
    </Address>
</Party>
```

#### ses_unbounded

Not required, but must be set for elements, that may occur more than once.

Contains a (space speparated) list of sub-element names, that would have maxOccurence="unbounded" in XSD. These elements must always be arrays in PHP classes/objects.

Example (Party.xml):

``` xml
<Party ses_unbounded="Address">
    <Id></Id>
    <Name></Name>
    <Address>
        <FirstName></FirstName>
        <LastName></LastName>
        <Street></Street>
        <PostalCode></PostalCode>
        <City></City>
        <Region></Region>
    </Address>
</Party>
```

#### ses_tree

Not required, but must be set for XML elements that should be serialized and deserialized without being defined by the generated PHP classes.

This is a space separated list of elements which are intended to contain arbitrary tree data. It is mainly used for the SesExtension field. Elements defined in this attribute are considered by serialization processes to contain any XML tree structure, which is directly converted into an PHP, accordingly.	

Example (Party.xml):

``` xml
<Party ses_tree="SesExtension">
    <Id></Id>
    <Name></Name>
    <Address>
        <FirstName></FirstName>
        <LastName></LastName>
        <Street></Street>
        <PostalCode></PostalCode>
        <City></City>
        <Region></Region>
    </Address>
    <SesExtension>
        <AdditionalId></AdditionalId>
    </SesExtension>
</Party>
```

### ses_desc

Not required, optional but recommended 

For documentation purposes. For example to document possible value-formats of a field.

This value will be written in the PHP docHeader of the member. (Not implemented, yet)

Example:

``` xml
<Example ses_desc="Default ISO date format">2013-01-11</Example>
```

Further ses\_... attributes could be implemented in the future. For example a "`ses_maxlength`", which could truncate the length of a field in the getter/setter, to limit a field to a certain size (e.g. 30 characters)

### Usage of the generator script

Workflow of the message generation:

- Create request and response XML files

    The XML content should be available in a concept documentation in form of example XML or at least as a common structured data description.
    - The file names MUST fit to the following scheme: `request|response.<messageName>.xml` whereas the content of `<messageName>` will be the base name for the generated files.
    - You MUST define one file for each: request AND response.
    - The files MUST contain well-formed XML code.
    - Examples:

    request.party.xml

    ``` xml
    <?xml version="1.0" encoding="UTF-8"?>
    <PartyRequest ses_type="cust:Party">
        <Party>
        </Party>
    </PartyRequest>
    ```

    Party.xml

    ``` xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Party ses_unbounded="Address">
        <Id></Id>
        <Name></Name>
        <Address>
            <FirstName></FirstName>
            <LastName></LastName>
            <Street></Street>
            <PostalCode></PostalCode>
            <City></City>
            <Region></Region>
        </Address>
    </Party>
    ```

    response.party.xml

    ``` xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ResponseParty>
        <Status></Status>
    </ResponseParty>
    ```

- Extend the XML according to the format, which is described above, if necessary.
- Upload the XML documents to the resources folder:
    - e.g. for standard messages:
        
          - `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xml/request.party.xml`
        
          - `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xml/response.party.xml`
    
    - e.g. for non standard messages
        
          - `src/Example/Bundle/ExampleBundle/Resources/xml/request.party.xml`
        
          - `src/Example/Bundle/ExampleBundle``/Resources/xml/response.party.xml`

- Start the generation (check for any error messages):
    
    - Standard schema for command line:

        ``` 
        php bin/console silversolutions:generatemessages --message <messageName> --sourceDir <path/to/src> --targetDir <path/to/target>
        ```

    - Example:

        ``` 
        php bin/console silversolutions:generatemessages --message party --sourceDir src/Example/Bundle/ExampleBundle/Resources/xml --targetDir src/Example/Bundle/ExampleBundle
        ```

    - The target and source directories MUST exist.
    - The target path MUST contain a 'src' directory. All directories beneath 'src' are considered as namespaces according to PSR-0.
    - The target path SHOULD be a Symfony bundle because all created sub-directories, like `Resources`, are intended to be used in a Symfony bundle.

- Using the `--force` parameter will overwrite all existing files. **Use this parameter with caution as it will also overwrite standard classes if used in the defined message**.
- The script will now generate all necessary files into the following directories. Depending of the type of the class, \<targetDir\> is the *SilversolutionsEshopBundle* (for ses standard classes) or the defined target directory (in our example `src/Example/Bundle/ExampleBundle`):
- `<targetDir>`
    - `Services`
        - `Factory`    
            - *Example files:*    
                - `PartyFactoryListener.php`
    - `Resources`
        - `config`
            - *Example files:*
                - `partyfactorylistener.service.yml`
                - `partymessage.message.yml`
            - in this directory, the configuration is stored, that must be included into the symfony application in order to activate the generated message factory.
            - to include this you:
                - might either import this file into the respective yaml-file in the app/config directory (e.g. config.yml). Example:

                ``` yaml
                imports:
                    - ...
                    - { resource: "@ExampleExampleBundle/Resources/config/partyfactorylistener.service.yml" }
                ```

                - or you copy paste the content of this file in the respective services.yml (or \*.xml, but then you have to reformat), Example:

                ``` yaml
                services:
                    ...
                    silver_messages.party_inquiry_listener:
                        class: "Example\\Bundle\\ExampleBundle\\Services\\Factory\\PartyFactoryListener"
                        tags:
                            - {name: kernel.event_listener, event: silver_erp.inquire_message, method: onMessageInquired}
                ```
                
    - `Entities`
        - `Messages`
            - `Document`
                - Example files
                    - `PartyMessage.php`

**Command line help**

``` bash
php bin/console silversolutions:generatemessages --help
Usage:
 silversolutions:generatemessages [--message="..."] [--sourceDir[="..."]] [--targetDir[="..."]] [--force]

Options:
 --message             The message, which should be converted
 --sourceDir           The optional target directory where to get XML
                       (default: /var/www/ez51/vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xml)
 --targetDir           The optional target directory where to create PHP
                       (default: /var/www/ez51/vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle)
 --force               Overwrite all existing PHP files
 --help (-h)           Display this help message.
 --quiet (-q)          Do not output any message.
 --verbose (-v)        Increase verbosity of messages.
 --version (-V)        Display this application version.
 --ansi                Force ANSI output.
 --no-ansi             Disable ANSI output.
 --no-interaction (-n) Do not ask any interactive question.
 --shell (-s)          Launch the shell.
 --process-isolation   Launch commands from shell as a separate process.
 --env (-e)            The Environment name. (default: "dev")
 --no-debug            Switches off debug mode.
 --siteaccess          SiteAccess to use for operations. If not provided, default siteaccess will be used

Help:
 The silversolutions:generatemessages command helps you generates new PHP objects from XML.
```

### Creation and working with messages

After creating messages via the MessageInquiryService, values of a document can be changes easily.

A simple code example, working with the example (Party) message: 

Code example for sending a RMA message:

``` php
<?
// In the context of a controller action you can pull service dependencies
// with $this->get('service_id');
 
/** @var \Silversolutions\Bundle\EshopBundle\Services\MessageInquiryService $inqService */
$inqService = $this->get('silver_erp.message_inquiry_service');
$message    = $inqService->inquireMessage(PartyFactoryListener::PARTY);
$request    = $message->getRequestDocument();
$request->Id->value = '23456';
$request->Name->value = 'Example & Co.';
$request->Address[] = new \Example\Bundle\ExampleBundle\Entities\Messages\Document\Address();
$request->Address[0]->FirstName->value = 'Peter';
$request->Address[0]->LastName->value = 'Smith';
// ... set further document values here ...
 
/** @var \Silversolutions\Bundle\EshopBundle\Services\Transport\AbstractMessageTransport $transport */
$transport  = $this->get('silver_erp.message_transport');
$response   = $transport->sendMessage($message)->getResponseDocument();
$status     = $response->Status->value;
// ... further work with the response via $response
```

## Development notes

- The converter is implemented within the *SilversolutionsEshopBundle* (Command/GenerateMessagesCommand.php and Generator/XmlToPhpGenerator.php)
- The target directory for standard elements / messages is the *SilversolutionsEshopBundle* and not configurable.
- The generated PHP classes and configurations are not validated automatically. This SHOULD be done manually after the generation.
