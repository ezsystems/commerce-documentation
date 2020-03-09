# EzHelperService

The purpose of this service is to provide some helper methods for eZ Platform regarding e.g. User Components or Translation Components.

!!! note "EzHelperService"

    Namespace: `Silversolutions\Bundle\ToolsBundle\ServicesService-ID: silver_tools.ez_helper`

Configuration

Parameters for EzHelperService are configurable:

!!! note "eZPlatform configResolver"

    See https://confluence.ez.no/display/EZP/Configuration?src=search#Configuration-DynamicconfigurationwiththeConfigResolver

Methods

General

|Method|Parameters|Returns|Description|
|--- |--- |--- |--- |
|clearVersions|ContentInfo $contentInfo|void|Clears the (archived) versions of the given $contentInfo to the required count of versions (set in the configuration)|

User Components

|Method|Parameters|Returns|Description|
|--- |--- |--- |--- |
|getCurrentUser|-|\eZ\Publish\API\Repository\Values\User\User|Returns the current user from the default context|
|updateUser|User $ezUser</br>array $fields = array()|\eZ\Publish\API\Repository\Values\Content\Content|Updates eZ User content|
|updateUserCustomAttributes|User $ezUser</br>array $fields = array()|\eZ\Publish\API\Repository\Values\Content\Content|Updates eZ User content which is custom (like "customer_number", "customer_profile_data", etc.)|
|getUserByUserId|$userId|\eZ\Publish\API\Repository\Values\User\User|Returns an eZUser by user ID|
|createUser|array $userData = array()</br>$defaultLanguage = 'eng-US'|\eZ\Publish\API\Repository\Values\User\User|Creates an eZ user|
|loginEzUser|\eZ\Publish\API\Repository\Values\User\User $user</br>\Symfony\Component\HttpFoundation\Session\Session $session</br>\Symfony\Component\HttpFoundation\Response $response|-|Login as an eZ User|
|isEzUserLoggedIn|Request $request|boolean|Returns true if eZ user is logged in|
|getAnonymousUser|-|\eZ\Publish\API\Repository\Values\User\User|Returns the anonymous user for the current eZ context|
|isEzUserAnonymous|\eZ\Publish\API\Repository\Values\User\User $user|boolean|Returns true if given user is anonymous user|
|findUser|$value</br>$searchAttribute = 'login'</br>$locationId = null|\eZ\Publish\API\Repository\Values\User\User or null|Returns an user object by an attribute</br>Default attribute is 'login', but other like 'email' are possible|

Siteaccess translation components

|Method|Parameters|Returns|Description|
|--- |--- |--- |--- |
|getCurrentLanguageCode()|-|null|string</br>e.g. ger-DE|Returns the current siteaccess language if set in the configuration, otherwise null|
|getTranslatedNameForEzContent()|\eZ\Publish\API\Repository\Values\Content\Content $ezContent|string|Returns the translated name of the eZ content|
|getTranslatedFieldForEzContent()|\eZ\Publish\API\Repository\Values\Content\Content $ezContent</br>string $fieldIdentifier|\eZ\Publish\API\Repository\Values\Content\Field|null|Returns the translated field of the eZ content|
|getUrlForSiteAccess()|string $urlPath|string</br>e.g. home -> /de/home|Modifies the given url to the url for the current siteaccess|
|getUserConf()|$key|string|Returns the parameter value from the configuration for given $key and namespace self::ST_EZ_HELPER_CREATE_USER|
|getEzContentByLocationId()|$ezLocationId|Content|Returns eZ content by location ID|
|getEzLocationByLocationId()|$ezLocationId|Content|Returns eZ location by location ID|
|getEzContentByLocation()|Location $ezLocation|Content|Returns eZ content by eZ location|
|getEzContentByContentId()|$ezContentId|Content|Returns eZ content by content ID|
|getEzTypeIdentifierByEzContent()|Content $ezContent|string|Returns the eZ type identifier for given eZ content|
|getEzContentFieldValue()|Content $ezContent</br>$fieldIdentifier</br>$languageCode</br>$fieldType|null|mixed|Returns a field value for given eZ content by identifier|
|getUrlAliasByUrl()|$url</br>$languageCode|\eZ\Publish\API\Repository\Values\Content\URLAlias</br>@throws \eZ\Publish\API\Repository\Exceptions\NotFoundException|Looks up the URLAlias for the given url.|
|getUrlAliasByLocation()|Location $ezLocation</br>$languageCode|null|\eZ\Publish\API\Repository\Values\Content\URLAlias|Returns the URL alias for a given eZ location|
