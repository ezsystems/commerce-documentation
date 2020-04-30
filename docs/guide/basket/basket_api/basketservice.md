# BasketService

The ID of the service is:

`silver_basket.basket_service`

Service methods:

|Method|Usage|Parameters|Return|
|--- |--- |--- |--- |
|getBasket|Returns basket of current user with state `new`|Request $request</br>string $state|Basket|
|storeBasket|Stores basket in the database. If necessary, the Price engine is initiated and Prices for Totals are calculated and stored.</br>Also the flag `$allProductsAvailable` is set here. If required the catalog elements are fetched and stored again</br>Parameter `$updateDateLastModified` is used to determine whether `dateLastModified` should be updated in the basket or not|Basket $basket</br>updateDateLastModified|Basket - stored $basket|
|getBasketGroupList|Get a list of used codes - `GroupCalcList` or `GroupOrderList` depending on `groupType`</br>This function isn't implemented yet.|Basket $basket, $groupType|array - list of used codes|
|mergeBasket|Merges 2 baskets</br>the lines of the additionalBasket are assigned to the baseBasket|Basket $baseBasket, Basket $additionalBasket|Basket - merged basket|
|copyBasket|Creates a new basket - with the state 'offered' - based on the given basket. All attributes will be copied.</br>The new basket will not be stored in the DB.|Basket $originBasket|Basket - copied basket|
|removeBasket|Removes basket from the DB. If $withAssigned is true, all baskets that are based on this basket are removed from the DB.|Basket $basket, $withAssigned = false||
|cleanUpBaskets|Removes all anonymous baskets from the storage that are older than given $datetime|\Datetime $datetime|int - count of the removed baskets</br>in failure null|
|getBasketsForType|Returns list of baskets for given type and status of current user.|Request $request, string $basketType, string $state|Basket[]|
|getBasketByUserId|Gets a basket from the DB by userId. If not found new Basket  is returned- but not stored in DB.|$userId, $type, $state, $name = null, $splittingCode = null|Basket - found or new basket|
|getBasketBySessionId|Gets a basket from the DB by sessionId. If not found new Basket  is returned- but not stored in DB.|$sessionId, $type, $state, $name = null, $splittingCode = null|Basket - found or new basket|
|addBasketLineToBasket|Adds BasketLine to the Basket and stores the Basket in the DB. Is used for throwing and intercepting events.|Basket $basket, $sku, $quantity, $variantCode = null||
|removeBasketLineFromBasket|Removes a BasketLine from a basket. Is used for throwing and intercepting events. Basket is not stored in DB.|Basket $basket, BasketLine $basketLine||
|updateBasketLineInBasket|Updates a BasketLine in Basket. Is used for throwing and intercepting events. Basket is not stored in DB.|Basket $basket, BasketLine $basketLine, $increase = false||
|createBasketLineForSku|Creates a new BasketLine for the given sku and variantCode|Basket $basket, $sku, $quantity, $variantCode = null|BasketLine|
|addBasketLineToStoredBasket|Adds a basket line to the basket.|Basket $basket</br>$basketType</br>$sku</br>$quantity</br>null\|string $variantCode</br>null\|array $dataMap||
|validateQuantity|Returns validated quantity as float|string $quantity|float|string|
|isValidQuantity|Returns true if quantity is valid.|string $quantity|bool|
|setBasketMessagesAsFlashMessages|This function sets all basket messages into session as flash bag messages the goal of this method is to store basket messages before e.g. redirect is done, so the basket messages are not lost|Basket $basket|void|
|setFlashMessagesAsBasketMessages|This function sets session flash bag messages into basket (if there are some) and deletes the flash bag messages from session|Basket $basket|void|

### Merge basket

How does the merging work?

When the user is in login process, the old session ID is stored in the session before the session ID is refreshed.

Then if the shop requires to get the current basket, first it is checked, if there are baskets to merge. For this the shop checks if there is a basket with the session id, that was stored in the session during the login process. If yes, it is merged with the basket of the current logged in user.
