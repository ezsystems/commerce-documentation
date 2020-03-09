# Downloads

Logic how to handle downloads is not implemented in silver-e.shop yet.

However there is a prepared entity to store the downloads data in the shop.

||||||
|--- |--- |--- |--- |--- |
|Field|Type in Shop|Type in DB|Required|Description|
|id|int|int|true|Unique ID|
|basketId|int|int|true|Id of the basket|
|userId|int|int|false|Id of the user|
|sku|string|string|true|Article number of the download|
|timeOfOrder|\DateTime|datetime|true|Timestamp of the order|
|quid|string|string|true|Unique hash 32 chars long
is created automatically|
