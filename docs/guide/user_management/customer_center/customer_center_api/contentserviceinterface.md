# ContentServiceInterface

`ContentServiceInterface` predefines methods for operations with Content items in the Customer center.

|Method|Description|
|--- |--- |
|`createCompanyObject(Party $customerParty)`|Creates a Company Content item using party data|
|`getCompanyObjectByCustomerNumber($customerNumber)`|Fetches the Company's Content item with the given customer number|
|`updateUserContentObject($userID, UserUpdateStruct $struct)`|Updates the User Content item with the provided parameters|
|`updateUserStatus($userID, UserUpdateStruct $struct)`|Updates user status with the provided parameters|
|`getUsers($customerNumber, array $parameters = [])`|Gets a list of User Content items with the given criteria|
|`getUsersByRoleName($customerNumber, $roleName)`|Gets a list of user Content items by customer number and `roleName`|
|`getMainContacts($customerNumber)`|Gets the list of main contacts for a given customer number|
|`getUser($customerNumber, $email)`|Gets a User Content item|
|`getUserById($ezUserId)`|Gets a User Content item using a given User ID|
|`getRolesOfUserObject(User $user)`|Returns list of user Roles of a User Content item|
|`getAvailableRoles($userId = null)`|Returns an array of Roles that are registered as Customer center roles|
|`loadRoleByIdentifierWithSudo($roleIdentifier)`|Returns a Role by identifier|

## CustomerCenterContentService

`CustomerCenterContentService` is the concrete implementation of `ContentServiceInterface`.

Service ID: `siso_customer_center.content_service`
