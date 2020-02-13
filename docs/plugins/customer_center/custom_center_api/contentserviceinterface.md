# ContentServiceInterface 

## Introduction

The purpose of this interface is to predifine methods for operations with eZ content object in a customer center.

## ContentServiceInterface

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>public function createCompanyObject(Party $customerParty);</code></pre></td>
<td><pre><code>Creates eZ Content company object using party data</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>public function getCompanyObjectByCustomerNumber($customerNumber);</code></pre></td>
<td><pre><code>Fetches the company&#39;s eZ content object with the given customer number</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>public function updateUserContentObject($userID, UserUpdateStruct $struct);</code></pre></td>
<td><pre><code>Updates user content object using provided parameters</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>public function updateUserStatus($userID, UserUpdateStruct $struct);</code></pre></td>
<td><pre><code>Updates user status using provided parameters</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>public function getUsers($customerNumber, array $parameters = []);</code></pre></td>
<td><pre><code>Gets a list of user content objects using given criteria</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>public function getUsersByRoleName($customerNumber, $roleName);</code></pre></td>
<td><pre><code>Gets a list of user content objects by customer no and roleName</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>public function getMainContacts($customerNumber);</code></pre></td>
<td><pre><code>Get the list of main contacts for a given customer number</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>public function getUser($customerNumber, $email);</code></pre></td>
<td><pre><code>Gets a user content object</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>public function getUserById($ezUserId);</code></pre></td>
<td><pre><code>Gets a user content object using a given eZ user ID</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>public function getRolesOfUserObject(User $user);</code></pre></td>
<td><pre><code>Returns list of user roles of user object</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>public function getAvailableRoles($userId = null);</code></pre></td>
<td><pre><code>Returns an array of roles that are registered as customer center roles</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>public function loadRoleByIdentifierWithSudo($roleIdentifier);</code></pre></td>
<td><pre><code>Return Role by identifier</code></pre></td>
</tr>
</tbody>
</table>
