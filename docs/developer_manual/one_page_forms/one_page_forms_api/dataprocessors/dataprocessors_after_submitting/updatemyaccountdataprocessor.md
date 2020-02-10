# UpdateMyAccountDataProcessor

The goal of this DataProcessor is to update the CustomerProfileData and other content object attributes (like email) in the User object in eZ, when the user was editing his account and actually made some changes (see [HasFormChangedDataProcessor](HasFormChangedDataProcessor_23560766.html)).

The changes will be stored in eZ only if the User has **no** customer number.

    ID of this service:
    ses.customer_profile_data.data_processor.update_buyer
    
    Corresponding Form:
    \Silversolutions\Bundle\EshopBundle\Form\Customer\MyAccount
