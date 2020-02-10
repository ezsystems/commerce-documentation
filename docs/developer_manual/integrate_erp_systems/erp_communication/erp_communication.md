#  ERP communication 

## Introduction

The ERP module is a very important part of eZ Commerce Advanced. By default the shop communicates with the Web-Connector which offers a realtime communication to different ERP systems such as Microsoft Dynamics NAV, AX and SAP. 

eZ Commerce Advanced uses the logic of an ERP system in different situations:

  - During login process - getting customer data 
  - In product detail page (configurable) - prices and stock infos
  - In basket 
  - In checkout
  - during registration - creating contacts oder customers
  - add on order history - getting documents from the ERP
  - product importer

 The shop comes with a predefined set of messages:

<table>
<thead>
<tr class="header">
<th>Message</th>
<th>Description</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td>calculate_sales_price</td>
<td>Calculates prices using the business logic of the ERP</td>
<td><br />
</td>
</tr>
<tr>
<td>createsalesorder</td>
<td>Creates an order</td>
<td><br />
</td>
</tr>
<tr>
<td>select_customer</td>
<td>Gets customer data from th ERP</td>
<td><br />
</td>
</tr>
<tr>
<td>select_contact</td>
<td>Gets contact data for a person from the ERP</td>
<td><br />
</td>
</tr>
<tr>
<td>create_contact</td>
<td>Create a contact in the ERP</td>
<td><br />
</td>
</tr>
<tr>
<td>updatecustomer</td>
<td>Updates a customer in the ERP</td>
<td><br />
</td>
</tr>
<tr>
<td>orderdetail</td>
<td>Gets details about an order</td>
<td><br />
</td>
</tr>
<tr>
<td>invoice_detail</td>
<td>Gets details about an invoice</td>
<td><br />
</td>
</tr>
<tr>
<td>delivery_note_detail</td>
<td>Gets details about a delivery note</td>
<td><br />
</td>
</tr>
<tr>
<td>orderlist</td>
<td>Gets an list of orders</td>
<td><br />
</td>
</tr>
<tr>
<td>invoice_list</td>
<td>Gets an list of invoice</td>
<td><br />
</td>
</tr>
<tr>
<td>delivery_note_list</td>
<td>Gets an list of delivery notes</td>
<td><br />
</td>
</tr>
<tr>
<td>creditmemolist</td>
<td>Gets an list of creditmemeos</td>
<td><br />
</td>
</tr>
<tr>
<td>creditmemodetail</td>
<td>Gets details about a creditmemo</td>
<td><br />
</td>
</tr>
<tr>
<td>readdeliveryaddress</td>
<td>Gets data of a delivery address for specified party ID</td>
<td><br />
</td>
</tr>
<tr>
<td>updatedeliveryaddress</td>
<td>Updates the ERP data of an existing delivery address</td>
<td><br />
</td>
</tr>
<tr>
<td>createdeliveryaddress</td>
<td>Creates a new delivery address for a specified party ID</td>
<td><br />
</td>
</tr>
<tr>
<td>deletedeliveryaddress</td>
<td>Deletes a specific delivery address</td>
<td><br />
</td>
</tr>
</tbody>
</table>

You will find the standard message defined in your installation in vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/messages.yml

Each bundle can extend messages or define own messages if required.

## General introduction into the concepts

The ERP module uses specifications based on XML definition files to model the data which is sent to the ERP and received by the ERP.  The request and response can be mapped used XSL files. This allows to adapt ERP-systems which are using non UBL based communication.

Learn more about the concept: 

[How does the shop communicate with the ERP](How-does-the-shop-communicate-with-the-ERP_23560630.html)

## Important configuration

Please make sure you have symlinks in app/Resources folder:

``` 
# If the Resources directory does not exist, create it first
mkdir app/Resources

cd app/Resources/
ln -s ../../vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase
 
# if you have local message please add this symlink as well!! 
ln -s ../../src/<NameSpace>/Bundle/CommonBundle/Resources/xsl
```

For more information about the ERP message mapping, please have a look at [ERP Component: Mapping](23560398.html).

## How to debug

Log message are logged to the database. A commandline tool offers a feature to check which messages where exchanged with the ERP system.

This command waits for the next message and displays the request and result as XML:

``` 
sudo -u _www php bin/console siso_tools:display_erp_log_command
 
2016-12-14 17:44:16 Start new entry in the Erp Log Table with ID 11441
Start ** mapping_type **
responseEnd ** mapping_type **
Start ** stylesheet **
app/Resources/xslbase/response.noop.xslEnd ** stylesheet **
Start ** input_xml **
<?xml version="1.0" encoding="UTF-8"?>
<OrderResponse>
  <SalesOrderID>0</SalesOrderID>
  <DocumentCurrencyCode/>
  <IssueDate>2016-12-14</IssueDate>
  <BuyerCustomerParty>
......
End ** input_xml **
Start ** output_xml **
<?xml version="1.0"?>
<OrderResponse>
  <SalesOrderID>0</SalesOrderID>
  <DocumentCurrencyCode/>
  <IssueDate>2016-12-14</IssueDate>
  <BuyerCustomerParty>
    <Party>
      <PartyName>
        <Name>Melanie Bourner</Name>
      </PartyName>
      <PartyIdentification>
        <ID>10000</ID>
      </PartyIdentification>
      <PostalAddress>
        <StreetName>F&#xE4;rberstra&#xDF;e 14</StreetName>
        <BuildingName/>
        <BuildingNumber/>
        <CityName>Berlin</CityName>
        <PostalZone>12</PostalZone>
        <CountrySubentity/>
        <AddressLine/>
        <Country>
          <IdentificationCode>DE</IdentificationCode>
        </Country>
      </PostalAddress>
    </Party>
  </BuyerCustomerParty>
...
End ** output_xml **
End new entry in the Erp Log Table.  Date: 2016-12-14 17:44:16
Previous information of the Measuring Point = "250_mapping"
```

If you want to search for a message you can use a fulltext search as well:

``` 
php bin/console siso_tools:display_erp_log_command --search-text 123456788
```

If you want to dump the latest 100 messages:

``` 
php bin/console siso_tools:display_erp_log_command --dump-last-messages 20 > /tmp/erp_messaages.txt
```

Removing messages from the Database:

This command removes messages older than 3 days. 

``` 
 php bin/console siso_tools:display_erp_log_command --delete-messages 3
```

## Important terms used in this document

ERP

**ERP** (Enterprise resource planning) is business [management](http://en.wikipedia.org/wiki/Management "Management") software—typically a suite of integrated applications—that a company can use to collect, store, manage and interpret data from many business activities, including:

  - [Product planning](http://en.wikipedia.org/wiki/Product_planning "Product planning"), cost
  - [Manufacturing](http://en.wikipedia.org/wiki/Manufacturing "Manufacturing") or service delivery
  - [Marketing](http://en.wikipedia.org/wiki/Marketing "Marketing") and sales
  - [Inventory management](http://en.wikipedia.org/wiki/Inventory_management "Inventory management")
  - Shipping and payment

*Source*: Wikipedia

Known ERP software packages:

  - Microsoft Dynamics NAV (former navision)
  - Microsoft Dynamics AX  
    
  - SAP

UBL

eZ Commerce uses UBL (Universal Business Language) as a standard for the communication between the ERP systems and the shop. UBL is used also for representing data in the shop such as the customer data. 

Extract from Wikipedia (<https://en.wikipedia.org/wiki/Universal_Business_Language>):

**Universal Business Language** (**UBL**) is a library of standard electronic [XML](https://en.wikipedia.org/wiki/XML "XML") business documents such as [purchase orders](https://en.wikipedia.org/wiki/Purchase_order "Purchase order") and [invoices](https://en.wikipedia.org/wiki/Invoice "Invoice"). UBL was developed by an [OASIS](https://en.wikipedia.org/wiki/OASIS_\(organization\) "OASIS (organization)") Technical Committee with participation from a variety of industry data standards organizations. UBL is designed to plug directly into existing business, legal, auditing, and records management practices.<sup>[\[1\]](https://en.wikipedia.org/wiki/Universal_Business_Language#cite_note-1)</sup> It is designed to eliminate the [re-keying](https://en.wikipedia.org/wiki/Data_entry_clerk "Data entry clerk") of data in existing fax- and paper-based business correspondence and provide an entry point into [electronic commerce](https://en.wikipedia.org/wiki/Electronic_commerce "Electronic commerce") for small and medium-sized businesses.

## How to use the API towards the ERP system in PHP

This example describes how to get product data from the ERP system:

``` 
$customerNumber = "10000";
$erpService = $this->getContainer()->get('silver_erp.facade');
$selectCustomerResponse = $erpService->selectCustomer($customerNumber);
echo "Customer-Data fpr customer with customer number:".$customerNumber;
print_r($selectCustomerResponse);
```

## How to retrieve products from ERP in a Symfony controller

``` 
/** @var WebConnectorErpService $importer */
$importer = $this->get('silver_erp.facade');
//Starting SKU
$startSku = '1..';
//Number of items to be retrieved (optional, default is 10)
$itemMaxCount = 30;
$items = $importer->selectItem($startSku, $itemMaxCount);
foreach($items as $products)
{
    foreach($products as $product)
    {
        //Prints the product's name
        echo (string)$product->ProductMeta->name->TextField->text->translations->translation[0]->value;
        //Prints the product's price
        echo (string)$product->ProductMeta->price->PriceField->price->price->value;
    }
}
```

Example output of selectItem method:

``` 
Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponse Object
(
    [Item] => Array
        (
            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItem Object
                (
                    [ProductMeta] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMeta Object
                        (
                            [priority] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [visibility] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [sku] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 3123
                                )
                            [manufacturerSku] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [ean] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [name] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetaname Object
                                (
                                    [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                        (
                                            [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                (
                                                    [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                        (
                                                            [translation] => Array
                                                                (
                                                                    [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                        (
                                                                            [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                (
                                                                                    [value] => ger-DE
                                                                                )
                                                                            [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                (
                                                                                    [value] => Gymnastikmatte RFM aufrollbar, blau 180x80x1,5cm
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                )
                                            [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                (
                                                    [value] => 
                                                )
                                        )
                                )
                            [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetatext Object
                                (
                                    [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                        (
                                            [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                (
                                                    [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                        (
                                                            [translation] => Array
                                                                (
                                                                )
                                                        )
                                                )
                                            [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                (
                                                    [value] => 
                                                )
                                        )
                                )
                            [type] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [isOrderable] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [price] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetaprice Object
                                (
                                    [PriceField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PriceField Object
                                        (
                                            [price] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PriceFieldprice Object
                                                (
                                                    [price] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [priceInclVat] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [priceExclVat] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [isVatPrice] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [vatPercent] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [currency] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [discount] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [campaign] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [source] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [quantityDiscount] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                            [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                (
                                                    [value] => 
                                                )
                                        )
                                )
                            [stock] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetastock Object
                                (
                                    [StockField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StockField Object
                                        (
                                            [stockNumeric] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => 
                                                )
                                            [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                (
                                                    [value] => 
                                                )
                                        )
                                )
                            [shortDescription] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetashortDescription Object
                                (
                                    [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                        (
                                            [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                (
                                                    [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                        (
                                                            [translation] => Array
                                                                (
                                                                )
                                                        )
                                                )
                                            [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                (
                                                    [value] => 
                                                )
                                        )
                                )
                            [sesLongDescription] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetasesLongDescription Object
                                (
                                    [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                        (
                                            [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                (
                                                    [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                        (
                                                            [translation] => Array
                                                                (
                                                                )
                                                        )
                                                )
                                            [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                (
                                                    [value] => 
                                                )
                                        )
                                )
                            [unit] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => STK
                                )
                            [packaging_unit] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [minOrderQuantity] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [maxOrderQuantity] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [allowedQuantity] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [vatCode] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 
                                )
                            [sesImage] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemProductMetasesImage Object
                                (
                                    [ImageField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageField Object
                                        (
                                            [alternativeText] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageFieldalternativeText Object
                                                (
                                                    [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageFieldalternativeTexttranslations Object
                                                        (
                                                            [translation] => Array
                                                                (
                                                                )
                                                        )
                                                )
                                            [fileName] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageFieldfileName Object
                                                (
                                                    [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                    [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ImageFieldfileNametranslations Object
                                                        (
                                                            [translation] => Array
                                                                (
                                                                )
                                                        )
                                                )
                                            [fileSize] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => 
                                                )
                                            [path] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => 
                                                )
                                            [id] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => 
                                                )
                                            [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                (
                                                    [value] => 
                                                )
                                        )
                                )
                        )
                    [GroupMeta] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMeta Object
                        (
                            [GroupLocations] => Array
                                (
                                    [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocations Object
                                        (
                                            [Location] => Array
                                                (
                                                    [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocation Object
                                                        (
                                                            [code] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [priority] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 1
                                                                )
                                                            [visibility] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 1
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocationtranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocationtranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => Gruppe ..
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [1] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocation Object
                                                        (
                                                            [code] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [priority] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 1
                                                                )
                                                            [visibility] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 1
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocationtranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemGroupMetaGroupLocationsLocationtranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => Kategorie_..
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                )
                                        )
                                )
                        )
                    [Datamap] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamap Object
                        (
                            [Image] => Array
                                (
                                )
                            [Array] => Array
                                (
                                    [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_long_description
                                                )
                                            [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                (
                                                    [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [1] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_attributes
                                                )
                                            [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                (
                                                    [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                        (
                                                                                            [value] => Array
                                                                                                (
                                                                                                    [Attributes] => Array
                                                                                                        (
                                                                                                            [0] => Array
                                                                                                                (
                                                                                                                    [Code] => ARTIKELART
                                                                                                                    [Label] => Artikelart
                                                                                                                    [Value] => Lagerartikel
                                                                                                                )
                                                                                                            [1] => Array
                                                                                                                (
                                                                                                                    [Code] => EINHEIT
                                                                                                                    [Label] => Einheit
                                                                                                                    [Value] => STK
                                                                                                                )
                                                                                                            [2] => Array
                                                                                                                (
                                                                                                                    [Code] => FARBE
                                                                                                                    [Label] => Farben
                                                                                                                    [Value] => BLAU
                                                                                                                )
                                                                                                            [3] => Array
                                                                                                                (
                                                                                                                    [Code] => KRED.-ART-NR.
                                                                                                                    [Label] => Lieferanten Artikelnr.
                                                                                                                    [Value] => 03625232
                                                                                                                )
                                                                                                            [4] => Array
                                                                                                                (
                                                                                                                    [Code] => LIEFERANT
                                                                                                                    [Label] => Lieferant
                                                                                                                    [Value] => Rehaforum Medical GmbH
                                                                                                                )
                                                                                                            [5] => Array
                                                                                                                (
                                                                                                                    [Code] => MWST
                                                                                                                    [Label] => Mehrwertsteuer
                                                                                                                    [Value] => 19 %
                                                                                                                )
                                                                                                        )
                                                                                                )
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [2] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_scaled_prices
                                                )
                                            [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                (
                                                    [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [3] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_documents
                                                )
                                            [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                (
                                                    [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [4] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_videos
                                                )
                                            [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                (
                                                    [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [5] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapArray Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_images
                                                )
                                            [ArrayField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayField Object
                                                (
                                                    [array] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarray Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ArrayFieldarraytranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                )
                            [File] => Array
                                (
                                )
                            [Price] => Array
                                (
                                )
                            [Stock] => Array
                                (
                                )
                            [TextLine] => Array
                                (
                                    [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_short_description
                                                )
                                            [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                (
                                                    [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => 180x80x1,5cm
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [1] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_onrequest
                                                )
                                            [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                (
                                                    [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [2] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_topproduct
                                                )
                                            [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                (
                                                    [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [3] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_variant_attr_1
                                                )
                                            [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                (
                                                    [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [4] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_variant_attr_2
                                                )
                                            [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                (
                                                    [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [5] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_variant_master_sku
                                                )
                                            [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                (
                                                    [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => 
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                    [6] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\ItemTransferResponseItemDatamapTextLine Object
                                        (
                                            [identifier] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                (
                                                    [value] => ses_weight
                                                )
                                            [TextField] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextField Object
                                                (
                                                    [text] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtext Object
                                                        (
                                                            [encoding] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                (
                                                                    [value] => 
                                                                )
                                                            [translations] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslations Object
                                                                (
                                                                    [translation] => Array
                                                                        (
                                                                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TextFieldtexttranslationstranslation Object
                                                                                (
                                                                                    [language] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => ger-DE
                                                                                        )
                                                                                    [value] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                                                                        (
                                                                                            [value] => 2
                                                                                        )
                                                                                )
                                                                        )
                                                                )
                                                        )
                                                    [SesExtension] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\TreeObject Object
                                                        (
                                                            [value] => 
                                                        )
                                                )
                                        )
                                )
                            [TextBlock] => Array
                                (
                                )
                            [Int] => Array
                                (
                                )
                            [Float] => Array
                                (
                                )
                            [Bool] => Array
                                (
                                )
                        )
                )
  
```

For information about mapping see [Guide - how to get product data from the ERP](Guide---how-to-get-product-data-from-the-ERP_23561027.html)

## FAQ

Solution for Exception The message object could not be determined. Probably no respective service was registered to the message identifier: "SelectItemMessage"

 When the message has been generated a new yml file has been produced.

The yml file has to be included. usually all yml files generated by the ERP command line script will be defined in the parameters.yml file in your bundle.

This file has to be loaded e.g. in your app/config/config.yml file.

## Cookbook

Find recipes in the guides:

  - [Guide - how to get product data from the ERP](Guide---how-to-get-product-data-from-the-ERP_23561027.html)
  - [How to create a new ERP message (example: ItemTransfer)](23560266.html)
  - [Usage of UBL](Usage-of-UBL_23560640.html)

## Technical details

[If you want to learm more about the underlying concept please check the section ERP Components](ERP-Components_23560387.html)

## Configuration

 [ERP Configuration](ERP-Configuration_23560389.html)
