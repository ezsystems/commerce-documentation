#  FormProcessorServices 

## CheckUserEmailFormProcessor  

Form processor that checks if user with given email address already exists in the company structure.  

### Configuration  

**Service ID**

```
siso_customer_center.processor.check_user_email
```

## AddUserInEzFormProcessor

Form processor that creates a new eZ user content object out of the posted form data. Besides the user's type basic fields, any additional data is stored within the customer profile data.

### Configuration

**Service ID**

```
siso_customer_center.processor.add_user_in_ez 
```

#  CreateContactInErpProcessor

If this processor is active, it will perform the CreateContact request to the ERP. This processor only creates the contact in the ERP system and sets CUSTOMER\_NUMBER and CONTACT\_NUMBER within the $lastResult array. If further processing is necessary, subsequent processors must take care of it.

###  Configuration  

**Service ID**

``` 
siso_customer_center.processor.create_contact_in_erp
```

# SaveProfileInSessionProcessor

Saves the CustomerProfileData in session if it's the current user and was define by a previous form processor.

### Configuration  

**Service ID**

``` 
siso_customer_center.processor.save_profile_in_session
```

# StoreUserInEzFormProcessor

Processor for storage of changed data in eZ.

### Configuration  

**Service ID**

``` 
siso_customer_center.processor.store_user_form_in_ez
```

# UpdateUserRolesFormProcessor

Processor tu update user roles in eZ.

### Configuration  

**Service ID**

``` 
siso_customer_center.processor.update_user_roles
```

## ValidateCustomerAndContactNumberProcessor

 Processor which checks given customer and contact number in ERP for validity and adds both to the $lastResponse array. FormDataProcessorException is thrown in case of any failed validation. Thus, any subsequent processor will not be reached.

### Configuration  

**Service ID**

``` 
siso_customer_center.processor.validate_customer_and_contact_number
```
