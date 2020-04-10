# UpdateBuyerDataProcessor

The goal of this DataProcessor is to update the CustomerProfileData in the User object in eZ, when User was editing his buyer address and he really made some changes (see [HasFormChangedDataProcessor](hasformchangeddataprocessor.md)).

The changes will be stored in eZ only if the User has no customer number.

ID of this service:

`ses.customer_profile_data.data_processor.update_buyer`

Corresponding form:

`\Silversolutions\Bundle\EshopBundle\Form\Customer\Buyer`
