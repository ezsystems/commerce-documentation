# TokenService

## Call the TokenService inside the Controller

The ID of the service is:

`silver_tools.token_service`

If you have no possibility to inject the service, you can use the container:

`$tokenService = $this->container->get('silver_tools.token_service');`

## Service Methods

|Method|Parameters|Return|Usage|
|--- |--- |--- |--- |
|createToken|int $userId,</br>\stdClass $netData,</br>int $validUntil,</br>string $actionServiceId,</br>string $actionServiceMethod,</br>string $tokenType,</br>boolean $persist = true|Token $token|creates a token with given parameters.</br>If persist = true, token will be stored in the database immidiately|
|loadToken|string $userToken|Token $token|fetch token from the database. If token is not valid InvalidTokenException is thrown.|
|invalidateToken|string $userToken|boolean|calls loadToken and removes token from the database.|
|storeToken|Token $token||stores token in the database.|
|removeToken|Token $token||removes token from the database|
|isTokenValid|string $userToken|boolean|returns true if token is valid|
