<a name="GbaseSocial"></a>

## GbaseSocial
**Kind**: global class  

* [GbaseSocial](#GbaseSocial)
    * [new GbaseSocial()](#new_GbaseSocial_new)
    * [.vkListPurchases(skip, limit, callback)](#GbaseSocial+vkListPurchases)
    * [.vkConsumePurchase(purchaseNum, callback)](#GbaseSocial+vkConsumePurchase)
    * [.okListPurchases(skip, limit, callback)](#GbaseSocial+okListPurchases)
    * [.okConsumePurchase(purchaseNum, callback)](#GbaseSocial+okConsumePurchase)

<a name="new_GbaseSocial_new"></a>

### new GbaseSocial()
A section of API holding mechanism of working with social networks VK.com and OK.ru

<a name="GbaseSocial+vkListPurchases"></a>

### gbaseSocial.vkListPurchases(skip, limit, callback)
Goblin Backend can receive in-application purchases through "purchase service callback" that being contacted by VK.com itself.
All of them are persisted and you can list them

**Kind**: instance method of [<code>GbaseSocial</code>](#GbaseSocial)  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| skip | <code>Number</code> | <code>0</code> | pagination skip |
| limit | <code>Number</code> | <code>20</code> | pagination limit |
| callback | <code>function</code> |  | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseSocial+vkConsumePurchase"></a>

### gbaseSocial.vkConsumePurchase(purchaseNum, callback)
Mark purchase as consumed. Purchases itself do nothing for player hence you should manage it on client-side.
Consume it and make some profile modifications(add gold or crystals). Consuming itself is atomic otherwise you'll
get a legit logic error with index 179. Remember that two different requests for consuming and profile modification
are not atomic so you need to implement some guarantees.

**Kind**: instance method of [<code>GbaseSocial</code>](#GbaseSocial)  

| Param | Type | Description |
| --- | --- | --- |
| purchaseNum | <code>Number</code> | num of particular purchase you can get from listing |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseSocial+okListPurchases"></a>

### gbaseSocial.okListPurchases(skip, limit, callback)
Goblin Backend can receive in-application purchases through "purchase service callback" that being contacted by OK.ru itself.
All of them are persisted and you can list them

**Kind**: instance method of [<code>GbaseSocial</code>](#GbaseSocial)  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| skip | <code>Number</code> | <code>0</code> | pagination skip |
| limit | <code>Number</code> | <code>20</code> | pagination limit |
| callback | <code>function</code> |  | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseSocial+okConsumePurchase"></a>

### gbaseSocial.okConsumePurchase(purchaseNum, callback)
Mark purchase as consumed. Purchases itself do nothing for player hence you should manage it on client-side.
Consume it and make some profile modifications(add gold or crystals). Consuming itself is atomic otherwise you'll
get a legit logic error with index 575. Remember that two different requests for consuming and profile modification
are not atomic so you need to implement some guarantees.

**Kind**: instance method of [<code>GbaseSocial</code>](#GbaseSocial)  

| Param | Type | Description |
| --- | --- | --- |
| purchaseNum | <code>Number</code> | num of particular purchase you can get from listing |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |