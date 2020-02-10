#  BasketService 

The ID of the service is:

    silver_basket.basket_service

Service methods:

<table>
<thead>
<tr class="header">
<th>Method</th>
<th>Usage</th>
<th>Parameters</th>
<th>Return</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>getBasket</code></pre></td>
<td><p>returns basket of current user with state new</p></td>
<td><p>Request $request<br />
string $state</p></td>
<td><p>Basket</p></td>
</tr>
<tr>
<td><pre><code>storeBasket</code></pre></td>
<td><p>Stores basket in the DB. If necessary, the PriceEngine is initiated and Prices for Totals are calculated and stored.</p>
<p>Also the flag $allProductsAvailable is set here. If required the catalog elements are fetched and stored again</p></td>
<td>Basket $basket</td>
<td>Basket - stored $basket</td>
</tr>
<tr>
<td><pre><code>getBasketGroupList</code></pre></td>
<td><div class="content-wrapper">
<p>get a list of used codes - GroupCalcList or GroupOrderList depending on groupType</p>

<p>This function isn´t implemented yet.</p>
</td>
<td>Basket $basket, $groupType</td>
<td>array - list of used codes</td>
</tr>
<tr>
<td><pre><code>mergeBasket</code></pre></td>
<td><p>merges 2 baskets</p>
<p>the lines of the additionalBasket are assigned to the baseBasket</p></td>
<td>Basket $baseBasket, Basket $additionalBasket</td>
<td>Basket - merged basket</td>
</tr>
<tr>
<td><pre><code>copyBasket</code></pre></td>
<td><p>creates a new basket - with the state 'offered' - based on the given basket. All attributes will be copied.</p>
<p>The new basket will not be stored in the DB.</p></td>
<td>Basket $originBasket</td>
<td>Basket - copied basket</td>
</tr>
<tr>
<td><pre><code>removeBasket</code></pre></td>
<td>remove basket from the DB. If $withAssigned is true, all baskets that are based on this basket are removed from the DB.</td>
<td>Basket $basket, $withAssigned = false</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>cleanUpBaskets</code></pre></td>
<td>removes all anonymous baskets from the storage that are older than given $datetime</td>
<td>\Datetime $datetime</td>
<td><p>int - count of the removed baskets</p>
<p>in failure null</p></td>
</tr>
<tr>
<td><pre><code>getBasketsForType</code></pre></td>
<td>returns list of baskets for given type and status of current user.</td>
<td><p>Request $request, string $basketType, string $state</p></td>
<td>Basket[]</td>
</tr>
<tr>
<td><pre><code>getBasketByUserId</code></pre></td>
<td>get a basket from the DB by userId. If not found new Basket  is returned- but not stored in DB.</td>
<td>$userId, $type, $state, $name = null, $splittingCode = null</td>
<td>Basket - found or new basket</td>
</tr>
<tr>
<td><pre><code>getBasketBySessionId</code></pre></td>
<td>get a basket from the DB by sessionId. If not found new Basket  is returned- but not stored in DB.</td>
<td>$sessionId, $type, $state, $name = null, $splittingCode = null</td>
<td>Basket - found or new basket</td>
</tr>
<tr>
<td><pre><code>addBasketLineToBasket</code></pre></td>
<td>adds BasketLine to the Basket and stores the Basket in the DB. Is used for throwing and intercepting events.</td>
<td>Basket $basket, $sku, $quantity, $variantCode = null</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>removeBasketLineFromBasket</code></pre></td>
<td>removes a BasketLine from a basket. Is used for throwing and intercepting events. Basket is not stored in DB.</td>
<td>Basket $basket, BasketLine $basketLine</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>updateBasketLineInBasket</code></pre></td>
<td>updates a BasketLine in Basket. Is used for throwing and intercepting events. Basket is not stored in DB.</td>
<td>Basket $basket, BasketLine $basketLine, $increase = false</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>createBasketLineForSku</code></pre></td>
<td>creates a new BasketLine for the given sku and variantCode</td>
<td>Basket $basket, $sku, $quantity, $variantCode = null</td>
<td>BasketLine</td>
</tr>
<tr>
<td><pre><code>addBasketLineToStoredBasket</code></pre></td>
<td>Adds a basket line to the basket.</td>
<td><p>Basket $basket<br />
$basketType<br />
$sku<br />
$quantity<br />
null|string $variantCode<br />
null|array $dataMap</p></td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>validateQuantity</code></pre></td>
<td>Returns validated quantity as float</td>
<td>string $quantity</td>
<td>float|string</td>
</tr>
<tr>
<td><pre><code>isValidQuantity</code></pre></td>
<td>Returns true if quantity is valid.</td>
<td>string $quantity</td>
<td>bool</td>
</tr>
<tr>
<td><pre><code>setBasketMessagesAsFlashMessages</code></pre></td>
<td><p>This function sets all basket messages into session as flash bag messages the goal of this method is to store basket messages before e.g. redirect is done, so the basket messages are not lost</p></td>
<td>Basket $basket</td>
<td>void</td>
</tr>
<tr>
<td><pre><code>setFlashMessagesAsBasketMessages</code></pre></td>
<td><p>This function sets session flash bag messages into basket (if there are some) and deletes the flash bag messages from session</p></td>
<td>Basket $basket</td>
<td>void</td>
</tr>
</tbody>
</table>

### Merge basket

How does the merging work?

When the user is in [login proces](/pages/createpage.action?spaceKey=EZC14&title=Login&linkCreation=true&fromPageId=23560232), the old session id is stored in the session before the session id is refreshed.

Then if the shop requires to get the current basket, first it is checked, if there are baskets to merge. For this the shop checks if there is a basket with the session id, that was stored in the session during the login process. If yes, it is merged with the basket of the current logged in user.
