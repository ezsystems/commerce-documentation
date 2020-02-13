# Basket functions 

## getBasket

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/basket/current (GET)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>returns the current basket of the user with header infos and order lines</p>
<p>standard service id: 'silver_basket.basket_service'</p></td>
</tr>
<tr class="odd">
<td>Request</td>
<td><div class="content-wrapper">
<p>empty</p>
<p><br />
</p>
</td>
</tr>
<tr class="even">
<td>Response</td>
<td><div class="content-wrapper">

<div class="codeHeader panelHeader pdl hide-border-bottom">
<strong></strong> <span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> <span class="expand-control-text">Expand source <span class="collapse-spinner-wrapper">

<div class="codeContent panelContent pdl hide-toolbar">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence; collapse: true" data-theme="Confluence"><code>&quot;Basket&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.Basket+json&quot;,
        &quot;basketId&quot;: 107,
        &quot;originId&quot;: null,
        &quot;erpOrderId&quot;: null,
        &quot;guid&quot;: null,
        &quot;state&quot;: &quot;new&quot;,
        &quot;type&quot;: &quot;basket&quot;,
        &quot;erpFailCounter&quot;: null,
        &quot;erpFailErrorLog&quot;: null,
        &quot;sessionId&quot;: &quot;haub3a7963sg9mj7vbtt2dj403&quot;,
        &quot;userId&quot;: null,
        &quot;basketName&quot;: null,
        &quot;invoiceParty&quot;: {
            &quot;_media-type&quot;: &quot;application\/vnd.ez.api.Party+json&quot;,
            &quot;PartyIdentification&quot;: [],
            &quot;PartyName&quot;: [
                {
                    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPartyName+json&quot;,
                    &quot;Name&quot;: &quot;Anke Test&quot;
                },
                {
                    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPartyName+json&quot;,
                    &quot;Name&quot;: null
                }
            ],
            &quot;PostalAddress&quot;: {
                &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPostalAddress+json&quot;,
                &quot;StreetName&quot;: &quot;Teststr. 11&quot;,
                &quot;AdditionalStreetName&quot;: null,
                &quot;BuildingNumber&quot;: null,
                &quot;CityName&quot;: &quot;Testingen&quot;,
                &quot;PostalZone&quot;: &quot;1111&quot;,
                &quot;CountrySubentity&quot;: &quot;Dummy&quot;,
                &quot;CountrySubentityCode&quot;: null,
                &quot;AddressLine&quot;: [],
                &quot;Country&quot;: {
                    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPostalAddressCountry+json&quot;,
                    &quot;IdentificationCode&quot;: &quot;NO&quot;,
                    &quot;Name&quot;: null
                },
                &quot;Department&quot;: null,
                &quot;SesExtension&quot;: {}
            },
            &quot;Contact&quot;: {
                &quot;_media-type&quot;: &quot;application\/vnd.ez.api.Contact+json&quot;,
                &quot;ID&quot;: null,
                &quot;Name&quot;: null,
                &quot;Telephone&quot;: null,
                &quot;Telefax&quot;: null,
                &quot;ElectronicMail&quot;: &quot;aka_test1@silversolutions.de&quot;,
                &quot;OtherCommunication&quot;: null,
                &quot;Note&quot;: null,
                &quot;SesExtension&quot;: {}
            },
            &quot;Person&quot;: {
                &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPerson+json&quot;,
                &quot;FirstName&quot;: null,
                &quot;FamilyName&quot;: null,
                &quot;Title&quot;: null,
                &quot;MiddleName&quot;: null,
                &quot;SesExtension&quot;: {}
            },
            &quot;SesExtension&quot;: {
                &quot;forceInvoice&quot;: false,
                &quot;customer_type&quot;: &quot;private&quot;
            }
        },
        &quot;deliveryParty&quot;: null,
        &quot;buyerParty&quot;: {
            &quot;_media-type&quot;: &quot;application\/vnd.ez.api.Party+json&quot;,
            &quot;PartyIdentification&quot;: [],
            &quot;PartyName&quot;: [
                {
                    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPartyName+json&quot;,
                    &quot;Name&quot;: &quot;Anke Test&quot;
                },
                {
                    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPartyName+json&quot;,
                    &quot;Name&quot;: null
                }
            ],
            &quot;PostalAddress&quot;: {
                &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPostalAddress+json&quot;,
                &quot;StreetName&quot;: &quot;Teststr. 11&quot;,
                &quot;AdditionalStreetName&quot;: null,
                &quot;BuildingNumber&quot;: null,
                &quot;CityName&quot;: &quot;Testingen&quot;,
                &quot;PostalZone&quot;: &quot;1111&quot;,
                &quot;CountrySubentity&quot;: &quot;Dummy&quot;,
                &quot;CountrySubentityCode&quot;: null,
                &quot;AddressLine&quot;: [],
                &quot;Country&quot;: {
                    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPostalAddressCountry+json&quot;,
                    &quot;IdentificationCode&quot;: &quot;NO&quot;,
                    &quot;Name&quot;: null
                },
                &quot;Department&quot;: null,
                &quot;SesExtension&quot;: {}
            },
            &quot;Contact&quot;: {
                &quot;_media-type&quot;: &quot;application\/vnd.ez.api.Contact+json&quot;,
                &quot;ID&quot;: null,
                &quot;Name&quot;: null,
                &quot;Telephone&quot;: null,
                &quot;Telefax&quot;: null,
                &quot;ElectronicMail&quot;: &quot;aka_test1@silversolutions.de&quot;,
                &quot;OtherCommunication&quot;: null,
                &quot;Note&quot;: null,
                &quot;SesExtension&quot;: {}
            },
            &quot;Person&quot;: {
                &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPerson+json&quot;,
                &quot;FirstName&quot;: null,
                &quot;FamilyName&quot;: null,
                &quot;Title&quot;: null,
                &quot;MiddleName&quot;: null,
                &quot;SesExtension&quot;: {}
            },
            &quot;SesExtension&quot;: {
                &quot;forceInvoice&quot;: false,
                &quot;customer_type&quot;: &quot;private&quot;
            }
        },
        &quot;remark&quot;: null,
        &quot;dateCreated&quot;: {
            &quot;_media-type&quot;: &quot;application\/vnd.ez.api.DateTime+json&quot;,
            &quot;_exception&quot;: {
                &quot;Name&quot;: &quot;eZ\\Publish\\Core\\REST\\Common\\Output\\Exceptions\\NoVisitorFoundException&quot;,
                &quot;Message&quot;: &quot;No visitor found for DateTime!&quot;
            }
        },
        &quot;dateLastModified&quot;: {
            &quot;_media-type&quot;: &quot;application\/vnd.ez.api.DateTime+json&quot;,
            &quot;_exception&quot;: {
                &quot;Name&quot;: &quot;eZ\\Publish\\Core\\REST\\Common\\Output\\Exceptions\\NoVisitorFoundException&quot;,
                &quot;Message&quot;: &quot;No visitor found for DateTime!&quot;
            }
        },
        &quot;shop&quot;: &quot;haugenbok&quot;,
        &quot;requirePriceUpdate&quot;: false,
        &quot;totals&quot;: {
            &quot;lines&quot;: {
                &quot;_media-type&quot;: &quot;application\/vnd.ez.api.BasketTotals+json&quot;,
                &quot;name&quot;: &quot;&quot;,
                &quot;totalNet&quot;: null,
                &quot;totalGross&quot;: null,
                &quot;vatList&quot;: {},
                &quot;groupType&quot;: &quot;order&quot;,
                &quot;currency&quot;: &quot;NOK&quot;
            },
            &quot;additionalLines&quot;: {
                &quot;_media-type&quot;: &quot;application\/vnd.ez.api.BasketTotals+json&quot;,
                &quot;name&quot;: &quot;&quot;,
                &quot;totalNet&quot;: null,
                &quot;totalGross&quot;: null,
                &quot;vatList&quot;: {},
                &quot;groupType&quot;: &quot;order&quot;,
                &quot;currency&quot;: &quot;NOK&quot;
            }
        },
        &quot;totalsSum&quot;: {
            &quot;_media-type&quot;: &quot;application\/vnd.ez.api.BasketTotals+json&quot;,
            &quot;name&quot;: &quot;&quot;,
            &quot;totalNet&quot;: null,
            &quot;totalGross&quot;: null,
            &quot;vatList&quot;: {},
            &quot;groupType&quot;: &quot;order&quot;,
            &quot;currency&quot;: &quot;NOK&quot;
        },
        &quot;currency&quot;: null,
        &quot;totalsSumNet&quot;: null,
        &quot;totalsSumGross&quot;: null,
        &quot;additionalLines&quot;: null,
        &quot;lines&quot;: [],
        &quot;dateLastPriceCalculation&quot;: {
            &quot;_media-type&quot;: &quot;application\/vnd.ez.api.DateTime+json&quot;,
            &quot;_exception&quot;: {
                &quot;Name&quot;: &quot;eZ\\Publish\\Core\\REST\\Common\\Output\\Exceptions\\NoVisitorFoundException&quot;,
                &quot;Message&quot;: &quot;No visitor found for DateTime!&quot;
            }
        },
        &quot;shippingMethod&quot;: &quot;expressDel&quot;,
        &quot;paymentMethod&quot;: &quot;invoice&quot;,
        &quot;paymentTransactionId&quot;: null,
        &quot;confirmationEmail&quot;: null,
        &quot;salesConfirmationEmail&quot;: null,
        &quot;allProductsAvailable&quot;: true,
        &quot;dataMap&quot;: {
            &quot;voucher_1234567&quot;: &quot;1234567&quot;
        }
    }</code></pre>
</td>
</tr>
</tbody>
</table>

## readBasketList

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/basket/header/{basket_type} (GET)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>returns all baskets of the given type of the user with header infos</p>
<p>standard service id: 'silver_basket.basket_service'</p></td>
</tr>
<tr class="odd">
<td>Request</td>
<td><div class="content-wrapper">
<p>empty</p>
<p><br />
</p>
</td>
</tr>
<tr class="even">
<td>Response</td>
<td><div class="content-wrapper">

<div class="codeHeader panelHeader pdl hide-border-bottom">
<strong></strong> <span class="collapse-source expand-control" style="display:none;"><span class="expand-control-icon icon"> <span class="expand-control-text">Expand source <span class="collapse-spinner-wrapper">

<div class="codeContent panelContent pdl hide-toolbar">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence; collapse: true" data-theme="Confluence"><code>{
  &quot;BasketListResponse&quot;: {
    &quot;_media-type&quot;: &quot;application/vnd.ez.api.BasketListResponse+json&quot;,
    &quot;basketList&quot;: [
      {
        &quot;_media-type&quot;: &quot;application/vnd.ez.api.stdClass+json&quot;,
        &quot;basketId&quot;: 1198,
        &quot;state&quot;: &quot;new&quot;,
        &quot;type&quot;: &quot;basket&quot;,
        &quot;userId&quot;: 26569,
        &quot;invoiceParty&quot;: {
          &quot;_media-type&quot;: &quot;application/vnd.ez.api.Party+json&quot;,
          &quot;PartyIdentification&quot;: [],
          &quot;PartyName&quot;: [
            {
              &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPartyName+json&quot;,
              &quot;Name&quot;: &quot;Anke Test&quot;
            },
            {
              &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPartyName+json&quot;,
              &quot;Name&quot;: null
            }
          ],
          &quot;PostalAddress&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPostalAddress+json&quot;,
            &quot;StreetName&quot;: &quot;610 N Elm DR&quot;,
            &quot;AdditionalStreetName&quot;: null,
            &quot;BuildingNumber&quot;: null,
            &quot;CityName&quot;: &quot;Beverly Hills&quot;,
            &quot;PostalZone&quot;: &quot;90210&quot;,
            &quot;CountrySubentity&quot;: &quot;CA&quot;,
            &quot;CountrySubentityCode&quot;: &quot;CA&quot;,
            &quot;AddressLine&quot;: [],
            &quot;Country&quot;: {
              &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPostalAddressCountry+json&quot;,
              &quot;IdentificationCode&quot;: &quot;US&quot;,
              &quot;Name&quot;: null
            },
            &quot;Department&quot;: null,
            &quot;SesExtension&quot;: {}
          },
          &quot;Contact&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.Contact+json&quot;,
            &quot;ID&quot;: null,
            &quot;Name&quot;: null,
            &quot;Telephone&quot;: null,
            &quot;Telefax&quot;: null,
            &quot;ElectronicMail&quot;: &quot;mal@silversolutions.de&quot;,
            &quot;OtherCommunication&quot;: null,
            &quot;Note&quot;: null,
            &quot;SesExtension&quot;: {}
          },
          &quot;Person&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPerson+json&quot;,
            &quot;FirstName&quot;: null,
            &quot;FamilyName&quot;: null,
            &quot;Title&quot;: null,
            &quot;MiddleName&quot;: null,
            &quot;SesExtension&quot;: {}
          },
          &quot;SesExtension&quot;: {
            &quot;forceInvoice&quot;: false,
            &quot;customer_type&quot;: &quot;private&quot;
          }
        },
        &quot;deliveryParty&quot;: {
          &quot;_media-type&quot;: &quot;application/vnd.ez.api.Party+json&quot;,
          &quot;PartyIdentification&quot;: [],
          &quot;PartyName&quot;: [
            {
              &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPartyName+json&quot;,
              &quot;Name&quot;: &quot;Anke Test&quot;
            },
            {
              &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPartyName+json&quot;,
              &quot;Name&quot;: null
            }
          ],
          &quot;PostalAddress&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPostalAddress+json&quot;,
            &quot;StreetName&quot;: &quot;610 N Elm DR&quot;,
            &quot;AdditionalStreetName&quot;: null,
            &quot;BuildingNumber&quot;: null,
            &quot;CityName&quot;: &quot;Beverly Hills&quot;,
            &quot;PostalZone&quot;: &quot;90210&quot;,
            &quot;CountrySubentity&quot;: &quot;CA&quot;,
            &quot;CountrySubentityCode&quot;: &quot;CA&quot;,
            &quot;AddressLine&quot;: [],
            &quot;Country&quot;: {
              &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPostalAddressCountry+json&quot;,
              &quot;IdentificationCode&quot;: &quot;US&quot;,
              &quot;Name&quot;: null
            },
            &quot;Department&quot;: null,
            &quot;SesExtension&quot;: {}
          },
          &quot;Contact&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.Contact+json&quot;,
            &quot;ID&quot;: null,
            &quot;Name&quot;: null,
            &quot;Telephone&quot;: null,
            &quot;Telefax&quot;: null,
            &quot;ElectronicMail&quot;: &quot;mal@silversolutions.de&quot;,
            &quot;OtherCommunication&quot;: null,
            &quot;Note&quot;: null,
            &quot;SesExtension&quot;: {}
          },
          &quot;Person&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPerson+json&quot;,
            &quot;FirstName&quot;: null,
            &quot;FamilyName&quot;: null,
            &quot;Title&quot;: null,
            &quot;MiddleName&quot;: null,
            &quot;SesExtension&quot;: {}
          },
          &quot;SesExtension&quot;: {
            &quot;forceInvoice&quot;: false,
            &quot;customer_type&quot;: &quot;private&quot;
          }
        },
        &quot;buyerParty&quot;: {
          &quot;_media-type&quot;: &quot;application/vnd.ez.api.Party+json&quot;,
          &quot;PartyIdentification&quot;: [],
          &quot;PartyName&quot;: [
            {
              &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPartyName+json&quot;,
              &quot;Name&quot;: &quot;Anke Test&quot;
            },
            {
              &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPartyName+json&quot;,
              &quot;Name&quot;: null
            }
          ],
          &quot;PostalAddress&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPostalAddress+json&quot;,
            &quot;StreetName&quot;: &quot;610 N Elm DR&quot;,
            &quot;AdditionalStreetName&quot;: null,
            &quot;BuildingNumber&quot;: null,
            &quot;CityName&quot;: &quot;Beverly Hills&quot;,
            &quot;PostalZone&quot;: &quot;90210&quot;,
            &quot;CountrySubentity&quot;: &quot;CA&quot;,
            &quot;CountrySubentityCode&quot;: &quot;CA&quot;,
            &quot;AddressLine&quot;: [],
            &quot;Country&quot;: {
              &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPostalAddressCountry+json&quot;,
              &quot;IdentificationCode&quot;: &quot;US&quot;,
              &quot;Name&quot;: null
            },
            &quot;Department&quot;: null,
            &quot;SesExtension&quot;: {}
          },
          &quot;Contact&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.Contact+json&quot;,
            &quot;ID&quot;: null,
            &quot;Name&quot;: null,
            &quot;Telephone&quot;: null,
            &quot;Telefax&quot;: null,
            &quot;ElectronicMail&quot;: &quot;mal@silversolutions.de&quot;,
            &quot;OtherCommunication&quot;: null,
            &quot;Note&quot;: null,
            &quot;SesExtension&quot;: {}
          },
          &quot;Person&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.PartyPerson+json&quot;,
            &quot;FirstName&quot;: null,
            &quot;FamilyName&quot;: null,
            &quot;Title&quot;: null,
            &quot;MiddleName&quot;: null,
            &quot;SesExtension&quot;: {}
          },
          &quot;SesExtension&quot;: {
            &quot;forceInvoice&quot;: false,
            &quot;customer_type&quot;: &quot;private&quot;
          }
        },
        &quot;remark&quot;: &quot;Test remark&quot;,
        &quot;dateCreated&quot;: {
          &quot;_media-type&quot;: &quot;application/vnd.ez.api.DateTime+json&quot;,
          &quot;date&quot;: &quot;2018-07-11 11:28:42.000000&quot;,
          &quot;timezone_type&quot;: 3,
          &quot;timezone&quot;: &quot;Europe/Berlin&quot;
        },
        &quot;dateLastModified&quot;: {
          &quot;_media-type&quot;: &quot;application/vnd.ez.api.DateTime+json&quot;,
          &quot;date&quot;: &quot;2018-07-31 13:16:09.000000&quot;,
          &quot;timezone_type&quot;: 3,
          &quot;timezone&quot;: &quot;Europe/Berlin&quot;
        },
        &quot;totals&quot;: [
          {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.BasketTotals+json&quot;
          },
          {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.BasketTotals+json&quot;
          }
        ],
        &quot;totalsSum&quot;: {
          &quot;_media-type&quot;: &quot;application/vnd.ez.api.BasketTotals+json&quot;
        },
        &quot;currency&quot;: &quot;USD&quot;,
        &quot;totalsSumNet&quot;: 784,
        &quot;totalsSumGross&quot;: 784,
        &quot;dateLastPriceCalculation&quot;: {
          &quot;_media-type&quot;: &quot;application/vnd.ez.api.DateTime+json&quot;,
          &quot;date&quot;: &quot;2018-07-31 12:48:58.000000&quot;,
          &quot;timezone_type&quot;: 3,
          &quot;timezone&quot;: &quot;Europe/Berlin&quot;
        },
        &quot;paymentMethod&quot;: &quot;invoice&quot;
      }
    ]
  }
}</code></pre>
</td>
</tr>
</tbody>
</table>

## updateBasketParty

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/basket/current/party/invoice (PATCH)</code></pre>
<pre><code>/api/ezp/v2/siso-rest/basket/current/party/delivery (PATCH)</code></pre>
<pre><code>/api/ezp/v2/siso-rest/basket/current/party/buyer (PATCH)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>validates party data and stores it in basket</p>
<p>Standard service id: 'siso_rest_api.basket_helper_service' (methods validateParty and storePartyInBasket)</p></td>
</tr>
<tr class="odd">
<td>Validation in standard</td>
<td><div class="content-wrapper">
<div class="confluence-information-macro confluence-information-macro-note">
<span class="aui-icon aui-icon-small aui-iconfont-warning confluence-information-macro-icon">

<p>Due to a bug in the Symfony Validation component, it is not possible to pass groups to sub-constraints of composite constraints. But the composite constraints themselfes can be configured with validation groups. This means that if the sub-constraints should be validated in different groups, the composite (in this example StringObject) must be duplicated with the respective combinations of groups. An example of this can be found in the ElectronicMail field of the Contact class below.</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\Party:
    properties:
        PartyName:
            - Valid: ~
        PostalAddress:
            - Valid: ~
        Contact:
            - Valid: ~
    constraints:
        - Callback: [Siso\RestApiBundle\Validator\Constraints\PartyValidator, validatePartyName]

Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PartyPartyName:
    properties:
       Name:
        - NotBlank: ~
        - Siso\RestApiBundle\Validator\Constraints\StringObject:
            - Length:
                min: 2
                max: 30
                minMessage: &#39;Please enter minimum {{ limit }} chars&#39;
                maxMessage: &#39;Maximum {{ limit }} chars&#39;

Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PartyPostalAddress:
    properties:
        StreetName:
            - Siso\RestApiBundle\Validator\Constraints\StringObject:
                - NotBlank: ~
                - Length:
                    min: 2
                    max: 30
                    minMessage: &#39;Please enter minimum {{ limit }} chars&#39;
                    maxMessage: &#39;Maximum {{ limit }} chars&#39;
        AdditionalStreetName:
            - Siso\RestApiBundle\Validator\Constraints\StringObject:
                - Length:
                    min: 2
                    max: 30
                    minMessage: &#39;Please enter minimum {{ limit }} chars&#39;
                    maxMessage: &#39;Maximum {{ limit }} chars&#39;
        CityName:
            - Siso\RestApiBundle\Validator\Constraints\StringObject:
                - NotBlank: ~
                - Length:
                    min: 2
                    max: 30
                    minMessage: &#39;Please enter minimum {{ limit }} chars&#39;
                    maxMessage: &#39;Maximum {{ limit }} chars&#39;
        PostalZone:
            - Siso\RestApiBundle\Validator\Constraints\StringObject:
                - NotBlank: ~
                - Siso\RestApiBundle\Validator\Constraints\Zip: ~
        CountrySubentity:
            - Siso\RestApiBundle\Validator\Constraints\StringObject:
                - Length:
                    min: 2
                    max: 30
                    minMessage: &#39;Please enter minimum {{ limit }} chars&#39;
                    maxMessage: &#39;Maximum {{ limit }} chars&#39;
        Country:
            - Valid: ~

Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PartyPostalAddressCountry:
    properties:
        IdentificationCode:
            - Siso\RestApiBundle\Validator\Constraints\StringObject:
                - NotBlank: ~

Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\Contact:
    properties:
        ElectronicMail:
            - Siso\RestApiBundle\Validator\Constraints\StringObject:
                groups: [&#39;invoice&#39;]
                constraints:
                    - NotBlank: ~
                    - Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints\Email: ~
            - Siso\RestApiBundle\Validator\Constraints\StringObject:
                groups: [&#39;Default&#39;]
                constraints:
                    - Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints\Email: ~
        Telephone:
            - Siso\RestApiBundle\Validator\Constraints\StringObject:
                - Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints\Phone: ~</code></pre>
</td>
</tr>
<tr class="even">
<td>Request</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;PartyData&quot;: {
    &quot;_data-type&quot;: &quot;invoice&quot;,
    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.Party+json&quot;,
    &quot;PartyIdentification&quot;: [],
    &quot;PartyName&quot;: [
        {
            &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPartyName+json&quot;,
            &quot;Name&quot;: &quot;Anke Test&quot;
        },
        {
            &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPartyName+json&quot;,
            &quot;Name&quot;: &quot;&quot;
        }
    ],
    &quot;PostalAddress&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPostalAddress+json&quot;,
        &quot;StreetName&quot;: &quot;Teststr. 11&quot;,
        &quot;AdditionalStreetName&quot;: &quot;&quot;,
        &quot;BuildingNumber&quot;: null,
        &quot;CityName&quot;: &quot;Testingen&quot;,
        &quot;PostalZone&quot;: &quot;1111&quot;,
        &quot;CountrySubentity&quot;: &quot;Dummy&quot;,
        &quot;CountrySubentityCode&quot;: null,
        &quot;AddressLine&quot;: [],
        &quot;Country&quot;: {
            &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPostalAddressCountry+json&quot;,
            &quot;IdentificationCode&quot;: &quot;NO&quot;,
            &quot;Name&quot;: null
        },
        &quot;Department&quot;: null,
        &quot;SesExtension&quot;: {}
    },
    &quot;Contact&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.Contact+json&quot;,
        &quot;ID&quot;: null,
        &quot;Name&quot;: null,
        &quot;Telephone&quot;: &quot;&quot;,
        &quot;Telefax&quot;: null,
        &quot;ElectronicMail&quot;: &quot;aka_test1@silversolutions.de&quot;,
        &quot;OtherCommunication&quot;: null,
        &quot;Note&quot;: null,
        &quot;SesExtension&quot;: {}
    },
    &quot;Person&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPerson+json&quot;,
        &quot;FirstName&quot;: null,
        &quot;FamilyName&quot;: null,
        &quot;Title&quot;: null,
        &quot;MiddleName&quot;: null,
        &quot;SesExtension&quot;: {}
    },
    &quot;SesExtension&quot;: {
        &quot;forceInvoice&quot;: false,
        &quot;customer_type&quot;: &quot;private&quot;
    }
}</code></pre>
</td>
</tr>
<tr class="odd">
<td>Response</td>
<td><div class="content-wrapper">
<strong>Success</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: []
    }</code></pre>
<strong>Error</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: {
            &quot;Party&quot;: {
                &quot;PartyName&quot;: [
                    {
                        &quot;Name&quot;: &quot;This value should not be blank.&quot;
                    }
                ],
                &quot;PostalAddress&quot;: {
                    &quot;StreetName&quot;: &quot;This value should not be blank.&quot;,
                    &quot;CityName&quot;: &quot;This value should not be blank.&quot;,
                    &quot;PostalZone&quot;: [
                        &quot;This value should not be blank.&quot;,
                        &quot;\&quot;\&quot; is not a valid ZIP code. For valid ZIP code use following pattern \&quot;1234\&quot;.&quot;
                    ]
                },
                &quot;Contact&quot;: {
                    &quot;ElectronicMail&quot;: &quot;This value should not be blank.&quot;
                }
            }
        }
    }
}</code></pre>
</td>
</tr>
</tbody>
</table>

## updateStoredBasket

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/basket/update-stored/{basketId} (POST)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>Adds lines to stored basket with id <strong>basketId</strong></p></td>
</tr>
<tr class="odd">
<td>Request</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
        &quot;BasketLineData&quot;: [{
            &quot;basketLineId&quot;: 900,
            &quot;lineNumber&quot;: 1,
            &quot;sku&quot;: &quot;000000000000001134&quot;,
            &quot;variantCode&quot;: null,
            &quot;productType&quot;: &quot;OrderableProductNode&quot;,
            &quot;quantity&quot;: 1,
            &quot;unit&quot;: null,
            &quot;price&quot;: 300,
            &quot;priceNet&quot;: 300,
            &quot;priceGross&quot;: 300,
            &quot;linePriceAmountNet&quot;: 300,
            &quot;linePriceAmountGross&quot;: 300,
            &quot;vat&quot;: 0,
            &quot;isIncVat&quot;: false,
            &quot;currency&quot;: &quot;EUR&quot;,
            &quot;catalogElement&quot;: {
                &quot;groupCalc&quot;: null,
                &quot;groupOrder&quot;: null,
                &quot;remark&quot;: null,
                &quot;remoteDataMap&quot;: {
                    &quot;isPriceValid&quot;: true,
                    &quot;lineRemark&quot;: null,
                    &quot;scaledPrices&quot;: null,
                    &quot;sapLineId&quot;: 10,
                    &quot;dlvDate&quot;: &quot;2018-08-14&quot;,
                    &quot;discount&quot;: 35,
                    &quot;hasDeliveryDateEmpty&quot;: false,
                    &quot;sapListPrice&quot;: {
                        &quot;price&quot;: 300,
                        &quot;currency&quot;: &quot;EUR&quot;,
                        &quot;stock&quot;: {
                            &quot;variantCharacteristics&quot;: null,
                            &quot;assignedLines&quot;: null
                        }
                    }

                }
            }
        },
            {
                &quot;basketLineId&quot;: 900,
                &quot;lineNumber&quot;: 1,
                &quot;sku&quot;: &quot;000000000000001134&quot;,
                &quot;variantCode&quot;: null,
                &quot;productType&quot;: &quot;OrderableProductNode&quot;,
                &quot;quantity&quot;: 1,
                &quot;unit&quot;: null,
                &quot;price&quot;: 300,
                &quot;priceNet&quot;: 300,
                &quot;priceGross&quot;: 300,
                &quot;linePriceAmountNet&quot;: 300,
                &quot;linePriceAmountGross&quot;: 300,
                &quot;vat&quot;: 0,
                &quot;isIncVat&quot;: false,
                &quot;currency&quot;: &quot;EUR&quot;,
                &quot;catalogElement&quot;: {
                    &quot;groupCalc&quot;: null,
                    &quot;groupOrder&quot;: null,
                    &quot;remark&quot;: null,
                    &quot;remoteDataMap&quot;: {
                        &quot;isPriceValid&quot;: true,
                        &quot;lineRemark&quot;: null,
                        &quot;scaledPrices&quot;: null,
                        &quot;sapLineId&quot;: 10,
                        &quot;dlvDate&quot;: &quot;2018-08-14&quot;,
                        &quot;discount&quot;: 35,
                        &quot;hasDeliveryDateEmpty&quot;: false,
                        &quot;sapListPrice&quot;: {
                            &quot;price&quot;: 300,
                            &quot;currency&quot;: &quot;EUR&quot;,
                            &quot;stock&quot;: {
                                &quot;variantCharacteristics&quot;: null,
                                &quot;assignedLines&quot;: null
                            }
                        }

                    }
                }
            }
        ]};
}</code></pre>
<p><br />
</p>
<p><br />
</p>
</td>
</tr>
<tr class="even">
<td>Response</td>
<td><div class="content-wrapper">
<strong>Success</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: {
            &quot;_success&quot;:&quot;&quot;
        }
    }</code></pre>
<strong>Error</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: {
            &quot;_error&quot;:[
                &quot;error_message&quot;
            ]
        }
    }</code></pre>
</td>
</tr>
</tbody>
</table>

## updateBasketShippingMethod

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/basket/current/shippingmethod (PATCH)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>Validates and stores shipping method in current basket</p>
<p>Standard service id: 'siso_rest_api.basket_helper_service' (methods validateShippingMethod and storeShippingMethodInBasket)</p></td>
</tr>
<tr class="odd">
<td>Validation in standard</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>Siso\RestApiBundle\Model\ShippingMethod:
    constraints:
        - Siso\RestApiBundle\Validator\Constraints\ShippingMethodConstraint: ~</code></pre>
</td>
</tr>
<tr class="even">
<td>Request</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ShippingMethodData&quot;:{
    &quot;shippingMethod&quot;:&quot;mail&quot;
}</code></pre>
</td>
</tr>
<tr class="odd">
<td>Response</td>
<td><div class="content-wrapper">
<strong>Success</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: []
    }</code></pre>
<strong>Error</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: [
            &quot;Object(Siso\\RestApiBundle\\Model\\ShippingMethod):\n    The value is not valid&quot;
        ]
    }</code></pre>
</td>
</tr>
</tbody>
</table>

## updateBasketPaymentMethod

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/basket/current/paymentmethod (PATCH)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>Validates and stores shipping method in current basket</p>
<p>Standard service id: 'siso_rest_api.basket_helper_service' (methods validatePaymentMethod and storePaymentMethodInBasket)</p></td>
</tr>
<tr class="odd">
<td>Validation in standard</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>Siso\RestApiBundle\Model\PaymentMethod:
    constraints:
        - Siso\RestApiBundle\Validator\Constraints\PaymentMethodConstraint: ~</code></pre>
</td>
</tr>
<tr class="even">
<td>Request</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;PaymentMethodData&quot;:{
    &quot;paymentMethod&quot;:&quot;invoice&quot;
}</code></pre>
</td>
</tr>
<tr class="odd">
<td>Response</td>
<td><div class="content-wrapper">
<strong>Success</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: []
    }</code></pre>
<strong>Error</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: [
            &quot;Object(Siso\\RestApiBundle\\Model\\PaymentMethod):\n    The value is not valid&quot;
        ]
    }</code></pre>
</td>
</tr>
</tbody>
</table>

## updateBasketVoucher

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/basket/current/voucher (PATCH)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>Valitates and stores the voucher code in current. If an empty field is sent, the current voucher will be deleted.</p>
<p>Standard service id: 'siso_rest_api.basket_helper_service' (method validateAndStoreVoucherCode)</p></td>
</tr>
<tr class="odd">
<td>Request</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;VoucherData&quot;:{
    &quot;voucherCode&quot;:&quot;1234567&quot;
}</code></pre>
</td>
</tr>
<tr class="even">
<td>Response</td>
<td><div class="content-wrapper">
<strong>Success</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: []
    }</code></pre>
<strong>Error</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: [
            &quot;Object(Siso\\RestApiBundle\\Model\\Voucher):\n    The value is not valid&quot;
        ]
    }</code></pre>
</td>
</tr>
</tbody>
</table>

## AddToBasket

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/basket/current/lines (POST)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>Call to add one or more items to the current basket.</p></td>
</tr>
<tr class="odd">
<td>Request</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;BasketLineData&quot;:[
        {
            &quot;sku&quot;: &quot;9788202544683&quot;,
            &quot;quantity&quot;: 3,
            &quot;isVariant&quot;: 0,
            &quot;variantCode&quot;: &#39;&#39;
        },
        {
            &quot;sku&quot;:&quot;9788210056116&quot;,
            &quot;quantity&quot;: 1,
            &quot;isVariant&quot;: 0,
            &quot;variantCode&quot;: &#39;&#39;
        }
    ]

}</code></pre>
</td>
</tr>
<tr class="even">
<td>Response</td>
<td><div class="content-wrapper">
<strong>Success</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: []
    }
}</code></pre>
<strong>Error (example)</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: [
            &quot;Product with the sku: 9788210056115 not found&quot;
        ]
    }
}</code></pre>
</td>
</tr>
</tbody>
</table>

## UpdateBasket

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/basket/update (POST)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>Call to update one or more items to the current basket.</p>
<p>If the quantity is 0, the line is removed from the basket</p>
<p>Currently only these properties are updated:</p>
<ul>
<li>quantity</li>
<li>RemoteDataMap</li>
<li>remark</li>
</ul></td>
</tr>
<tr class="odd">
<td>Request</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;BasketLineData&quot;:[
    {
        &quot;_media-type&quot;: &quot;application/vnd.ez.api.BasketLine+json&quot;,
        &quot;basketLineId&quot;: 122,
        &quot;lineNumber&quot;: 3,
        &quot;sku&quot;: &quot;000000000000001134&quot;,
        &quot;variantCode&quot;: null,
        &quot;productType&quot;: &quot;OrderableProductNode&quot;,
        &quot;quantity&quot;: 2,
        &quot;unit&quot;: null,
        &quot;price&quot;: 392,
        &quot;priceNet&quot;: 392,
        &quot;priceGross&quot;: 392,
        &quot;linePriceAmountNet&quot;: 392,
        &quot;linePriceAmountGross&quot;: 392,
        &quot;vat&quot;: 0,
        &quot;isIncVat&quot;: false,
        &quot;currency&quot;: &quot;USD&quot;,
        &quot;catalogElement&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.OrderableProductNode+json&quot;,
            &quot;sku&quot;: &quot;000000000000001134&quot;,
            &quot;manufacturerSku&quot;: &quot;&quot;,
            &quot;ean&quot;: &quot;4053913000014&quot;,
            &quot;type&quot;: null,
            &quot;isOrderable&quot;: null,
            &quot;price&quot;: {
                &quot;_media-type&quot;: &quot;application/vnd.ez.api.PriceField+json&quot;
            },
            &quot;customerPrice&quot;: null,
            &quot;scaledPrices&quot;: null,
            &quot;stock&quot;: null,
            &quot;shortDescription&quot;: {
                &quot;_media-type&quot;: &quot;application/vnd.ez.api.TextBlockField+json&quot;
            },
            &quot;longDescription&quot;: {
                &quot;_media-type&quot;: &quot;application/vnd.ez.api.TextBlockField+json&quot;
            },
            &quot;specifications&quot;: null,
            &quot;imageList&quot;: {
                &quot;0&quot;: {
                    &quot;_media-type&quot;: &quot;application/vnd.ez.api.ImageField+json&quot;
                },
                &quot;1&quot;: {
                    &quot;_media-type&quot;: &quot;application/vnd.ez.api.ImageField+json&quot;
                },
                &quot;2&quot;: {
                    &quot;_media-type&quot;: &quot;application/vnd.ez.api.ImageField+json&quot;
                }
            },
            &quot;minOrderQuantity&quot;: null,
            &quot;maxOrderQuantity&quot;: null,
            &quot;allowedQuantity&quot;: null,
            &quot;packagingUnit&quot;: null,
            &quot;unit&quot;: null,
            &quot;vatCode&quot;: &quot;VATUNIT&quot;,
            &quot;name&quot;: &quot;2/2-way-piston-operated angle-seat valve&quot;,
            &quot;text&quot;: &quot;&quot;,
            &quot;image&quot;: {
                &quot;_media-type&quot;: &quot;application/vnd.ez.api.ImageField+json&quot;
            },
            &quot;path&quot;: null,
            &quot;url&quot;: &quot;/process-and-control-valves/shut-off-valves-on-off/angle-seat-valves/pneumatic/1134&quot;,
            &quot;parentElementIdentifier&quot;: &quot;1000015&quot;,
            &quot;identifier&quot;: &quot;1005576&quot;,
            &quot;cacheIdentifier&quot;: null,
            &quot;checkProperties&quot;: null
        },
        &quot;groupCalc&quot;: null,
        &quot;groupOrder&quot;: null,
        &quot;remark&quot;: null,
        &quot;remoteDataMap&quot;: {
            &quot;isPriceValid&quot;: true,
            &quot;lineRemark&quot;: null,
            &quot;scaledPrices&quot;: null,
            &quot;hasDeliveryDateEmpty&quot;: true
        },
        &quot;variantCharacteristics&quot;: null,
        &quot;assignedLines&quot;: null
    },
    {
        &quot;_media-type&quot;: &quot;application/vnd.ez.api.BasketLine+json&quot;,
        &quot;basketLineId&quot;: 123,
        &quot;lineNumber&quot;: 4,
        &quot;sku&quot;: &quot;000000000000134590&quot;,
        &quot;variantCode&quot;: null,
        &quot;productType&quot;: &quot;OrderableProductNode&quot;,
        &quot;quantity&quot;: 3,
        &quot;unit&quot;: null,
        &quot;price&quot;: 623,
        &quot;priceNet&quot;: 623,
        &quot;priceGross&quot;: 623,
        &quot;linePriceAmountNet&quot;: 623,
        &quot;linePriceAmountGross&quot;: 623,
        &quot;vat&quot;: 0,
        &quot;isIncVat&quot;: false,
        &quot;currency&quot;: &quot;USD&quot;,
        &quot;catalogElement&quot;: {
            &quot;_media-type&quot;: &quot;application/vnd.ez.api.OrderableProductNode+json&quot;,
            &quot;sku&quot;: &quot;000000000000134590&quot;,
            &quot;manufacturerSku&quot;: &quot;&quot;,
            &quot;ean&quot;: &quot;4053913025352&quot;,
            &quot;type&quot;: null,
            &quot;isOrderable&quot;: null,
            &quot;price&quot;: {
                &quot;_media-type&quot;: &quot;application/vnd.ez.api.PriceField+json&quot;
            },
            &quot;customerPrice&quot;: null,
            &quot;scaledPrices&quot;: null,
            &quot;stock&quot;: null,
            &quot;shortDescription&quot;: {
                &quot;_media-type&quot;: &quot;application/vnd.ez.api.TextBlockField+json&quot;
            },
            &quot;longDescription&quot;: {
                &quot;_media-type&quot;: &quot;application/vnd.ez.api.TextBlockField+json&quot;
            },
            &quot;specifications&quot;: null,
            &quot;imageList&quot;: {
                &quot;0&quot;: {
                    &quot;_media-type&quot;: &quot;application/vnd.ez.api.ImageField+json&quot;
                },
                &quot;1&quot;: {
                    &quot;_media-type&quot;: &quot;application/vnd.ez.api.ImageField+json&quot;
                },
                &quot;2&quot;: {
                    &quot;_media-type&quot;: &quot;application/vnd.ez.api.ImageField+json&quot;
                }
            },
            &quot;minOrderQuantity&quot;: null,
            &quot;maxOrderQuantity&quot;: null,
            &quot;allowedQuantity&quot;: null,
            &quot;packagingUnit&quot;: null,
            &quot;unit&quot;: null,
            &quot;vatCode&quot;: &quot;VATUNIT&quot;,
            &quot;name&quot;: &quot;2/2-way-solenoid valve; servo-piston&quot;,
            &quot;text&quot;: &quot;&quot;,
            &quot;image&quot;: {
                &quot;_media-type&quot;: &quot;application/vnd.ez.api.ImageField+json&quot;
            },
            &quot;path&quot;: null,
            &quot;url&quot;: &quot;/solenoid-valves/general-purpose-2-2-solenoids/134590&quot;,
            &quot;parentElementIdentifier&quot;: &quot;1000001&quot;,
            &quot;identifier&quot;: &quot;1006516&quot;,
            &quot;cacheIdentifier&quot;: null,
            &quot;checkProperties&quot;: null
        },
        &quot;groupCalc&quot;: null,
        &quot;groupOrder&quot;: null,
        &quot;remark&quot;: null,
        &quot;remoteDataMap&quot;: {
            &quot;isPriceValid&quot;: true,
            &quot;lineRemark&quot;: null,
            &quot;scaledPrices&quot;: null,
            &quot;hasDeliveryDateEmpty&quot;: true
        },
        &quot;variantCharacteristics&quot;: null,
        &quot;assignedLines&quot;: null
    }
]

}</code></pre>
</td>
</tr>
<tr class="even">
<td>Response</td>
<td><div class="content-wrapper">
<strong>Success</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: []
    }
}</code></pre>
<strong>Error (example)</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;ValidationResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ValidationResponse+json&quot;,
        &quot;messages&quot;: [
            &quot;Product with the sku: 9788210056115 not found&quot;
        ]
    }
}</code></pre>
</td>
</tr>
</tbody>
</table>
**Success**

``` 
"ValidationResponse": {
        "_media-type": "application\/vnd.ez.api.ValidationResponse+json",
        "messages": []
    }
```
