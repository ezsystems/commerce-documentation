# BaseOperation

This class is used as a base class for all *operation services*. It is abstract and holds dependencies to services, which are commonly needed by business services.

Currently the following services are injected into this base class.

| service                                                              | base class attribute |
| -------------------------------------------------------------------- | -------------------- |
| \Silversolutions\Bundle\EshopBundle\Services\LogService         | $logger              |
| \Silversolutions\Bundle\TranslationBundle\Services\TransService | $transService        |

The service ID is: `ses_eshop.business_api.base`
