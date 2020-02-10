#  TokenService 

## Call the TokenService inside the Controller

The ID of the service is:

    silver_tools.token_service

If you have no possibility to inject the service, you can use the container:

`$tokenService = $``this``->container->get(``'silver_tools.token_service'``);`

## Service Methods

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Parameters</th>
<th>Return</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td>createToken</td>
<td><p>int $userId,</p>
<p>\stdClass $netData,</p>
<p>int $validUntil,</p>
<p>string $actionServiceId,</p>
<p>string $actionServiceMethod,</p>
<p>string $tokenType,</p>
<p>boolean $persist = true</p></td>
<td>Token $token</td>
<td><p>creates a token with given parameters.</p>
<p>If persist = true, token will be stored in the database immidiately</p></td>
</tr>
<tr>
<td>loadToken</td>
<td>string $userToken</td>
<td>Token $token</td>
<td>fetch token from the database. If token is not valid InvalidTokenException is thrown.</td>
</tr>
<tr>
<td>invalidateToken</td>
<td>string $userToken</td>
<td>boolean</td>
<td>calls loadToken and removes token from the database.</td>
</tr>
<tr>
<td>storeToken</td>
<td>Token $token</td>
<td> </td>
<td>stores token in the database.</td>
</tr>
<tr>
<td>removeToken</td>
<td>Token $token</td>
<td> </td>
<td>removes token from the database</td>
</tr>
<tr>
<td>isTokenValid</td>
<td>string $userToken</td>
<td>boolean</td>
<td>returns true if token is valid</td>
</tr>
</tbody>
</table>
