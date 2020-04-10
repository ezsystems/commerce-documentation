# Customer profile data events

## CustomerProfileDataEventInterface

`\Silversolutions\Bundle\EshopBundle\Event\CustomerProfileData\CustomerProfileDataEventInterface`

The general interface for any event which is thrown in customer profile data services is thrown.

## AbstractCustomerProfileDataEvent

`\Silversolutions\Bundle\EshopBundle\Event\CustomerProfileData\AbstractCustomerProfileDataEvent`

Abstract event for any customer profile data event provides helper methods like `setCustomerProfileData()` to make a profile available for an event listener.

## EzErpCustomerProfileDataEvent

`\Silversolutions\Bundle\EshopBundle\Event\CustomerProfileData\EzErpCustomerProfileDataEvent`

Concrete event which also provides the eZ user, e.g. for fallback purposes when the ERP did not respond (see [Customer profile data listeners](customer_profile_data_listeners.md))

List of events, dispatched by `\Silversolutions\Bundle\EshopBundle\Services\CustomerProfileData\EzErpCustomerProfileDataService`:

|Event id|Description|
|--- |--- |
|ses_ez_erp_customer_profile_data_pre_fetch|Thrown before any data is fetched from storage(s)|
|ses_ez_erp_customer_profile_data_post_fetch|Thrown after all data is fetched from storage(s)|
|ses_ez_erp_customer_profile_data_pre_erp_customer_fetch|Thrown before ERP customer data is fetched|
|ses_ez_erp_customer_profile_data_post_erp_customer_fetch_success|Thrown after ERP customer data is successfully fetched|
|ses_ez_erp_customer_profile_data_post_erp_customer_fetch_fail|Thrown after ERP customer data fetching failed|
|ses_ez_erp_customer_profile_data_pre_erp_contact_fetch|Thrown before ERP contact data is fetched|
|ses_ez_erp_customer_profile_data_post_erp_contact_fetch_success|Thrown after ERP contact data fetching successed|
|ses_ez_erp_customer_profile_data_post_erp_contact_fetch_fail|Thrown after ERP contact data fetching failed|
|ses_ez_erp_customer_profile_data_pre_get_customer|Thrown before customer data is returned|
|ses_ez_erp_customer_profile_data_pre_save_customer|Thrown before customer data is saved|
