#  Customer profile data services 

<table>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Classes
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>\Silversolutions\Bundle\EshopBundle\Services\CustomerProfileData\CustomerProfileDataServiceInterface</code></td>
<td>The general interface for any customer profile data service.</td>
</tr>
<tr>
<td><code>\Silversolutions\Bundle\EshopBundle\Services\CustomerProfileData\AbstractCustomerProfileDataService</code></td>
<td>Abstract customer profile data service implementation which provides helper methods for deriving services.</td>
</tr>
<tr>
<td><code>\Silversolutions\Bundle\EshopBundle\Services\CustomerProfileData\EzErpCustomerProfileDataService</code></td>
<td>Concrete customer profile data service implementation using ERP as source for customer and contact data. Also uses eZ platform as source for account data and target for fallback data.</td>
</tr>
<tr>
<td><code>\Silversolutions\Bundle\EshopBundle\Services\CustomerProfileData\DeprecatedCustomerMappingService</code></td>
<td>Concrete customer profile data service which implements the deprecated <code>AbstracCustomerService</code> to use with eZ Commerce components, which depend on the deprecated model.</td>
</tr>
</tbody>
</table>

# Using customer profile data services in source code

Please do **not** use the [customerservice](http://confluence.extranet.silversolutions.de:8090/display/EX/Customer+profile+data+services) in any place, that can not access the session. An example will be a CLI tool, or processes that are happing in backgroud - like sending out the order if customer payed via payment service provider.

To use a customer profile data service, e.g. the `EzErpCustomerProfileDataService`, you might inject the service into your method resp. using the Symfony DIC.

``` 
/** @var \Silversolutions\Bundle\EshopBundle\Services\CustomerProfileData\CustomerProfileDataServiceInterface $customerProfileDataService */
$customerProfileDataService = $container->get('ses.customer_profile_data.ez_erp');
 
/** @var \Silversolutions\Bundle\EshopBundle\Model\CustomerProfileData\CustomerProfileData $profile */
$profile = $customerProfileDataService->getCustomerProfileData();
 
$phoneNumberContact = $profile->sesUser->contact->phoneNumber;
```

![](attachments/23560911/23563985.png)
