#  EzHelperService 

The purpose of this service is to provide some helper methods for eZ Platform regarding e.g. User Components or Translation Components.

EzHelperService

Namespace: Silversolutions\\Bundle\\ToolsBundle\\ServicesService-ID: silver\_tools.ez\_helper

Configuration

Parameters for EzHelperService are configurable:

eZPlatform configResolver

Siehe <https://confluence.ez.no/display/EZP/Configuration?src=search#Configuration-DynamicconfigurationwiththeConfigResolver>

Methods

General

<table>
<thead>
<tr class="header">
<th>Method</th>
<th>Parameters</th>
<th>Returns</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>clearVersions</td>
<td>ContentInfo $contentInfo</td>
<td>void</td>
<td><div class="content-wrapper">
<p>Clears the (archived) versions of the given $contentInfo to the required count of versions (set in the configuration)</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code> </code></pre>
</td>
</tr>
</tbody>
</table>

User Components

<table>
<thead>
<tr class="header">
<th>Method</th>
<th>Parameters</th>
<th>Returns</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>getCurrentUser</code></pre></td>
<td>-</td>
<td>\eZ\Publish\API\Repository\Values\User\User</td>
<td>Returns the current user from the default context</td>
</tr>
<tr>
<td><pre><code>updateUser</code></pre></td>
<td><p>User $ezUser</p>
<p>array $fields = array()</p></td>
<td>\eZ\Publish\API\Repository\Values\Content\Content</td>
<td>Updates eZ User content</td>
</tr>
<tr>
<td><pre><code>updateUserCustomAttributes</code></pre></td>
<td><p>User $ezUser</p>
<p>array $fields = array()</p></td>
<td>\eZ\Publish\API\Repository\Values\Content\Content</td>
<td>Updates eZ User content which is custom (like "customer_number", "customer_profile_data", etc.)</td>
</tr>
<tr>
<td><pre><code>getUserByUserId</code></pre></td>
<td>$userId</td>
<td>\eZ\Publish\API\Repository\Values\User\User</td>
<td>Returns an eZUser by user ID</td>
</tr>
<tr>
<td><pre><code>createUser</code></pre></td>
<td><p>array $userData = array()</p>
<p>$defaultLanguage = 'eng-US'</p></td>
<td>\eZ\Publish\API\Repository\Values\User\User</td>
<td>Creates an eZ user</td>
</tr>
<tr>
<td><pre><code>loginEzUser</code></pre></td>
<td><p>\eZ\Publish\API\Repository\Values\User\User $user</p>
<p>\Symfony\Component\HttpFoundation\Session\Session $session</p>
<p>\Symfony\Component\HttpFoundation\Response $response</p></td>
<td>-</td>
<td>Login as an eZ User</td>
</tr>
<tr>
<td><pre><code>isEzUserLoggedIn</code></pre></td>
<td>Request $request</td>
<td>boolean</td>
<td>Returns true if eZ user is logged in</td>
</tr>
<tr>
<td><pre><code>getAnonymousUser</code></pre></td>
<td>-</td>
<td>\eZ\Publish\API\Repository\Values\User\User</td>
<td>Returns the anonymous user for the current eZ context</td>
</tr>
<tr>
<td><pre><code>isEzUserAnonymous</code></pre></td>
<td>\eZ\Publish\API\Repository\Values\User\User $user</td>
<td>boolean</td>
<td>Returns true if given user is anonymous user</td>
</tr>
<tr>
<td><pre><code>findUser</code></pre></td>
<td><p>$value</p>
<p>$searchAttribute = 'login'</p>
<p>$locationId = null</p></td>
<td>\eZ\Publish\API\Repository\Values\User\User or null</td>
<td><p>Returns an user object by an attribute</p>
<p>Default attribute is 'login', but other like 'email' are possible</p>
<p><br />
</p></td>
</tr>
</tbody>
</table>

Siteaccess translation components

<table>
<thead>
<tr class="header">
<th>Method</th>
<th>Parameters</th>
<th>Returns</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>getCurrentLanguageCode()</code></pre></td>
<td>-</td>
<td><p>null|string</p>
<p>e.g. ger-DE</p></td>
<td>Returns the current siteaccess language if set in the configuration, otherwise null</td>
</tr>
<tr>
<td><pre><code>getTranslatedNameForEzContent()</code></pre></td>
<td>\eZ\Publish\API\Repository\Values\Content\Content $ezContent</td>
<td>string</td>
<td>Returns the translated name of the eZ content</td>
</tr>
<tr>
<td><pre><code>getTranslatedFieldForEzContent()</code></pre></td>
<td><p>\eZ\Publish\API\Repository\Values\Content\Content $ezContent</p>
<p>string $fieldIdentifier</p></td>
<td>\eZ\Publish\API\Repository\Values\Content\Field|null</td>
<td>Returns the translated field of the eZ content</td>
</tr>
<tr>
<td><pre><code>getUrlForSiteAccess()</code></pre></td>
<td>string $urlPath</td>
<td><p>string</p>
<p>e.g. home -&gt; /de/home</p></td>
<td>Modifies the given url to the url for the current siteaccess</td>
</tr>
<tr>
<td><pre><code>getUserConf()</code></pre></td>
<td>$key</td>
<td>string</td>
<td><p>Returns the parameter value from the configuration for given $key and namespace self::ST_EZ_HELPER_CREATE_USER</p></td>
</tr>
<tr>
<td><pre><code>getEzContentByLocationId()</code></pre></td>
<td>$ezLocationId</td>
<td>Content</td>
<td>Returns eZ content by location ID</td>
</tr>
<tr>
<td><pre><code>getEzLocationByLocationId()</code></pre></td>
<td>$ezLocationId</td>
<td>Content</td>
<td>Returns eZ location by location ID</td>
</tr>
<tr>
<td><pre><code>getEzContentByLocation()</code></pre></td>
<td>Location $ezLocation</td>
<td>Content</td>
<td>Returns eZ content by eZ location</td>
</tr>
<tr>
<td><pre><code>getEzContentByContentId()</code></pre></td>
<td><p>$ezContentId</p></td>
<td>Content</td>
<td>Returns eZ content by content ID</td>
</tr>
<tr>
<td><pre><code>getEzTypeIdentifierByEzContent()</code></pre></td>
<td>Content $ezContent</td>
<td>string</td>
<td>Returns the eZ type identifier for given eZ content</td>
</tr>
<tr>
<td><pre><code>getEzContentFieldValue()</code></pre></td>
<td><p>Content $ezContent<br />
$fieldIdentifier<br />
$languageCode<br />
$fieldType</p></td>
<td>null|mixed</td>
<td>Returns a field value for given eZ content by identifier</td>
</tr>
<tr>
<td><pre><code>getUrlAliasByUrl()</code></pre></td>
<td>$url<br />
$languageCode</td>
<td>\eZ\Publish\API\Repository\Values\Content\URLAlias<br />
<br />
@throws \eZ\Publish\API\Repository\Exceptions\NotFoundException</td>
<td>Looks up the URLAlias for the given url.</td>
</tr>
<tr>
<td><pre><code>getUrlAliasByLocation()</code></pre></td>
<td>Location $ezLocation<br />
$languageCode</td>
<td>null|\eZ\Publish\API\Repository\Values\Content\URLAlias</td>
<td>Returns the URL alias for a given eZ location</td>
</tr>
</tbody>
</table>
