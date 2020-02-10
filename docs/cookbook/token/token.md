#  Token 

The Token can be used for different scenario. The eCommerce solution uses the token system in the registration process in order to create the double-opt in possibility.

The Token-Service is able to generate a unique toke which will be valid until a given time. A universal controller will take care when a user tries to use the token and will trigger a service (actionServiceId)  and method (actionServiceMethod) which has been assigned to this token. 

### General

After a user registration a token is created a stored in the Database. The Token is valid for a specific amount of time. After the user clicked on the generated url, that he has received via email, the token is validated, fetched from the database and after the user has been activated, the token is removed from the database.

### Howto create a token

First you have to create a token. A token contains the

  - userId
  - Parameters 
  - the time until it is valid, 
  - a service and method which will be called if a customer uses the link to use the token

``` 
use \Silversolutions\Bundle\ToolsBundle\Entity\Token;
use \Silversolutions\Bundle\ToolsBundle\Services\TokenService;

$newsletterTokenData = New NewsletterTokenData(); // The object which shall be used later on when the token is used
$newsletterTokenData->setParams($newsletterParams);
$newsletterTokenData->setCustomerProfileData($customerProfileData);
$actionServiceMethod = 'subscribeNewsletter';
$actionService = 'siso_newsletter.newsletter.newsletter_service';
/** @var Token $token */
$token = $this->tokenService->createToken(
    $currentUser->id,
    $newsletterTokenData,
    $validUntil,
    $actionService,
    $actionServiceMethod,
    Token::TOKEN_TYPE_ONE_TIME
);

```

### How to use the token

There are several ways to use the token:

  - use the prepared token controller to call a service and method
  - Use it in your own Controller/Service to check a token

#### Check the token in own Service

The token can be checked using the tokenservice. It will return the parameters which has been stored when the token was created: 

``` 
$userToken = "12909dmsd912912"; // e.g. from the Request
/** @var Token $token */
$token = $this->tokenService->loadToken($userToken);

$serviceMethod = $token->getActionServiceMethod();
$tokenParameter = $token->getActionServiceMethodParameter();
```

#### The token Controller

The token can be used to distribute a link to a customer. The link contains the token as well:

/token/\<tokenid\>

### Example of a Service which is called when a customer uses the token link

``` 
// Service id: siso_newsletter.newsletter.newsletter_service
use Silversolutions\Bundle\EshopBundle\Model\CustomerProfileData\CustomerProfileData;
use Siso\Bundle\NewsletterBundle\Api\NewsletterInterface;
use Siso\Bundle\NewsletterBundle\Exception\AccessDeniedException;
use Siso\Bundle\NewsletterBundle\Exception\ParameterInvalidValueException;
use Siso\Bundle\NewsletterBundle\Exception\ParameterRequiredException;
use Siso\Bundle\NewsletterBundle\Exception\ServiceUnavailableException;
use Siso\Bundle\NewsletterBundle\Exception\WrongMethodException;

class NoNewsletterService implements NewsletterInterface
{

    /**
     * 
     *
     * @param array $params
     * @param CustomerProfileData $customerProfileData
     * @return array
     */
    public function subscribeNewsletter(array $params, CustomerProfileData $customerProfileData = null)
    {
    }
```

## Token (Entity)

Doctrine is used to stored the token in the database.

<table>
<thead>
<tr class="header">
<th>Silversolutions\Bundle\ToolsBundle\Entity\Token</th>
<th><br />
</th>
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td>Attribute</td>
<td>Type (PHP)</td>
<td>Mandatory</td>
<td>Usage</td>
</tr>
<tr>
<td>id</td>
<td>int</td>
<td>yes</td>
<td>primary key. Must be unique.</td>
</tr>
<tr>
<td>token</td>
<td>string (32 chars)</td>
<td>yes</td>
<td>generated token. Must be unique.</td>
</tr>
<tr>
<td>userId</td>
<td>int</td>
<td>yes</td>
<td>Id of the user, who has created the token. Can be anonymous.</td>
</tr>
<tr>
<td>dateCreated</td>
<td>\DateTime</td>
<td>yes</td>
<td>Timestamp of token creation.</td>
</tr>
<tr>
<td>validUntil</td>
<td>\DateTime</td>
<td>yes</td>
<td>Timestamp until the token is valid.</td>
</tr>
<tr>
<td>lastAccess</td>
<td>\DateTime</td>
<td>yes</td>
<td>Timestamp when the token has been requested last</td>
</tr>
<tr>
<td>lockTimeout</td>
<td>\DateTime</td>
<td>no</td>
<td>Timestamp how long the token is locked. In this time after last access the token can not be used.</td>
</tr>
<tr>
<td>actionServiceId</td>
<td>string</td>
<td>yes</td>
<td>ID of the service that will be invoked</td>
</tr>
<tr>
<td>actionServiceMethod</td>
<td>string</td>
<td>yes</td>
<td>Name of the Service method, that will be invoked</td>
</tr>
<tr>
<td>actionServiceMethodParameter</td>
<td>eZ\Publish\Core\Repository\Values\User\User </td>
<td>yes</td>
<td>Parameter for the service method</td>
</tr>
<tr>
<td>tokenType</td>
<td>string</td>
<td>yes</td>
<td>type of the token. Currently only <strong>one_time_token</strong>. Only defined types are allowed.</td>
</tr>
</tbody>
</table>
