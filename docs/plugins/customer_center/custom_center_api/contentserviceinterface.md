# ContentServiceInterface

## Introduction

The purpose of this interface is to predefine methods for operations with eZ content object in a customer center.

## ContentServiceInterface

|Method|Description|
|--- |--- |
|public function createCompanyObject(Party $customerParty);|Creates eZ Content company object using party data|
|public function getCompanyObjectByCustomerNumber($customerNumber);|Fetches the company's eZ content object with the given customer number|
|public function updateUserContentObject($userID, UserUpdateStruct $struct);|Updates user content object using provided parameters|
|public function updateUserStatus($userID, UserUpdateStruct $struct);|Updates user status using provided parameters|
|public function getUsers($customerNumber, array $parameters = []);|Gets a list of user content objects using given criteria|
|public function getUsersByRoleName($customerNumber, $roleName);|Gets a list of user content objects by customer no and roleName|
|public function getMainContacts($customerNumber);|Get the list of main contacts for a given customer number|
|public function getUser($customerNumber, $email);|Gets a user content object|
|public function getUserById($ezUserId);|Gets a user content object using a given eZ user ID|
|public function getRolesOfUserObject(User $user);|Returns list of user roles of user object|
|public function getAvailableRoles($userId = null);|Returns an array of roles that are registered as customer center roles|
|public function loadRoleByIdentifierWithSudo($roleIdentifier);|Return Role by identifier|
