# Customer profile data listeners

## ListenerInterface

`\Silversolutions\Bundle\EshopBundle\EventListener\CustomerProfileData\ListenerInterface`

The interface defines, which methods all listeners must have (e.g. `onPreFetch()`, `onPostFetch()` events)

## EzErpListener

`\Silversolutions\Bundle\EshopBundle\EventListener\CustomerProfileData\EzErpListener`

Concrete event listener implementation. Provides ERP related listeners, e.g. for ERP response success or fail.

`onPostErpCustomerFetchFail()`:

- Event: `ses_ez_erp_customer_profile_data_post_erp_customer_fetch_fail`
- Loads customer data from the respective eZ User content object if it could not be retrieved from the ERP.

`onPreFetch()`:

- No implementation

`onPostFetch()`:

No implementation
