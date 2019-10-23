<a name="GbasePvpApi"></a>

## GbasePvpApi
**Kind**: global class  

* [GbasePvpApi](#GbasePvpApi)
    * [new GbasePvpApi()](#new_GbasePvpApi_new)
    * [.checkBattleNoSearch(callback)](#GbasePvpApi+checkBattleNoSearch)
    * [.dropMatchmaking(callback)](#GbasePvpApi+dropMatchmaking)
    * [.withOpponent(fromSegment, strategy, ranges, timeLimitSec, callback)](#GbasePvpApi+withOpponent)
    * [.withBotOpponent(ranges, callback)](#GbasePvpApi+withBotOpponent)
    * [.stopSearchingForOpponent()](#GbasePvpApi+stopSearchingForOpponent) ⇒ <code>\*</code>
    * [.beginVersusSelf(targetHumanId, callback)](#GbasePvpApi+beginVersusSelf)
    * [.beginOnAddressAndKey(pvpRoomData, callback)](#GbasePvpApi+beginOnAddressAndKey)
    * [.battlesList(skip, limit, onlyAuto, callback)](#GbasePvpApi+battlesList)

<a name="new_GbasePvpApi_new"></a>

### new GbasePvpApi()
A section of API holding everything needed to start PvP with real player or fictive opponent

<a name="GbasePvpApi+checkBattleNoSearch"></a>

### gbasePvpApi.checkBattleNoSearch(callback)
Player's matchmaking and pvp state is exclusive hence player can't play a few pvps at once. If you faced client crash and restart
you should run this method to check whatever player was at pvp before crash. In response you'll get a status and pvp data if has.
Provide this pvp data into beginOnAddressAndKey method to continue pvp.

**Kind**: instance method of [<code>GbasePvpApi</code>](#GbasePvpApi)  

| Param | Type | Description |
| --- | --- | --- |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbasePvpApi+dropMatchmaking"></a>

### gbasePvpApi.dropMatchmaking(callback)
Useful service method to force clean all matchmaking data. You can call it every time after proper finishing yet another pvp.

**Kind**: instance method of [<code>GbasePvpApi</code>](#GbasePvpApi)  

| Param | Type | Description |
| --- | --- | --- |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbasePvpApi+withOpponent"></a>

### gbasePvpApi.withOpponent(fromSegment, strategy, ranges, timeLimitSec, callback)
Search and begin pvp 1 versus 1. The gameplay will be encapsulated into "response.pvp" value as {GbasePvp}

**Kind**: instance method of [<code>GbasePvpApi</code>](#GbasePvpApi)  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| fromSegment | <code>String</code> |  | ratings segment to search among |
| strategy | <code>String</code> |  | if backend is configured to receive client-defined strategy, you can pick one from two available: GbaseApi.MATCHMAKING_STRATEGIES.BY_RATING and GbaseApi.MATCHMAKING_STRATEGIES.BY_LADDER |
| ranges | <code>GbaseRangePicker</code> |  | an instance with ranges of search. See test examples |
| timeLimitSec | <code>Number</code> | <code>60</code> | limit searching time in seconds. No guarantee that limit will be accurately equal to provided value because process is abrupt |
| callback | <code>function</code> |  | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbasePvpApi+withBotOpponent"></a>

### gbasePvpApi.withBotOpponent(ranges, callback)
Begin gameplay versus bot profile. Need backend to be configured with bots

**Kind**: instance method of [<code>GbasePvpApi</code>](#GbasePvpApi)  

| Param | Type | Description |
| --- | --- | --- |
| ranges | <code>GbaseRangePicker</code> | an instance with ranges of search. See test examples |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbasePvpApi+stopSearchingForOpponent"></a>

### gbasePvpApi.stopSearchingForOpponent() ⇒ <code>\*</code>
Stops search  No guarantee that limit will be accurately at exact moment because process is abrupt

**Kind**: instance method of [<code>GbasePvpApi</code>](#GbasePvpApi)  
<a name="GbasePvpApi+beginVersusSelf"></a>

### gbasePvpApi.beginVersusSelf(targetHumanId, callback)
Start gameplay player versus self based on pvp framework. Pipeline is the same and all messages will return to you.
It's useful to use coupled with cloud functions.

**Kind**: instance method of [<code>GbasePvpApi</code>](#GbasePvpApi)  

| Param | Type | Description |
| --- | --- | --- |
| targetHumanId | <code>Number</code> | some Human ID be provided to "pvpGeneratePayload" cloud function. Used to imitate gameplay versus some target opponent without his participate |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbasePvpApi+beginOnAddressAndKey"></a>

### gbasePvpApi.beginOnAddressAndKey(pvpRoomData, callback)
Continue pvp gameplay with some provided data. You can get it with method "checkBattleNoSearch"

**Kind**: instance method of [<code>GbasePvpApi</code>](#GbasePvpApi)  

| Param | Type | Description |
| --- | --- | --- |
| pvpRoomData | <code>Object</code> | pvp data from "checkBattleNoSearch" method response |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbasePvpApi+battlesList"></a>

### gbasePvpApi.battlesList(skip, limit, onlyAuto, callback)
Pvp gameplay may produce battle journal entry. It's made by calling function "appendBattleJournalPvp"
from cloud function "pvpGameOverHandler" or "pvpAutoCloseHandler".
It'll contain some details on battle plus schema-less data from cloud code programmer.

**Kind**: instance method of [<code>GbasePvpApi</code>](#GbasePvpApi)  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| skip | <code>Number</code> | <code>0</code> | pagination skip |
| limit | <code>Number</code> | <code>20</code> | pagination limit |
| onlyAuto | <code>boolean</code> |  | list only entries produced from cloud function "pvpAutoCloseHandler" |
| callback | <code>function</code> |  | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbasePvp"></a>

## GbasePvp
**Kind**: global class  

* [GbasePvp](#GbasePvp)
    * [new GbasePvp()](#new_GbasePvp_new)
    * [.myPing](#GbasePvp+myPing) ⇒ <code>Number</code>
    * [.opponentPing](#GbasePvp+opponentPing) ⇒ <code>Number</code>
    * [.isPaused](#GbasePvp+isPaused) ⇒ <code>boolean</code>
    * [.opponentPayload](#GbasePvp+opponentPayload) ⇒ <code>object</code>
    * [.startTimestamp](#GbasePvp+startTimestamp) ⇒ <code>Number</code>
    * [.randomSeed](#GbasePvp+randomSeed) ⇒ <code>Number</code>
    * [.meIsPlayerA](#GbasePvp+meIsPlayerA) ⇒ <code>boolean</code>
    * [.doConnect(payloadObject)](#GbasePvp+doConnect)
    * [.sendTurn(turnMessage)](#GbasePvp+sendTurn)
    * [.sendDirect(directMessage)](#GbasePvp+sendDirect)
    * [.forceDestroyClient()](#GbasePvp+forceDestroyClient)

<a name="new_GbasePvp_new"></a>

### new GbasePvp()
A pvp instance produced by [GbasePvpApi](#GbasePvpApi). It emits the next events: "progress"(representing connection progress, providing progress message),
"begin"(representing a start of gameplay. You can start your game after that), "error"(on any errors), "direct-message"(on direct message),
"turn-message"(on turn message), "model"(providing an information about model. Usually after reconnection),
"paused"(when game went on pause), "unpaused"(when game returns from pause),
"sync"(representing need to sync your local model or state with backend's. Not a frequent but usually if game is paused and client thinks that you lost some messages),
"finish"(representing finish of pvp match. GbasePvp instance is now done)

<a name="GbasePvp+myPing"></a>

### gbasePvp.myPing ⇒ <code>Number</code>
**Kind**: instance property of [<code>GbasePvp</code>](#GbasePvp)  
**Returns**: <code>Number</code> - - My ping in ms  
<a name="GbasePvp+opponentPing"></a>

### gbasePvp.opponentPing ⇒ <code>Number</code>
**Kind**: instance property of [<code>GbasePvp</code>](#GbasePvp)  
**Returns**: <code>Number</code> - - Opponent's ping in ms  
<a name="GbasePvp+isPaused"></a>

### gbasePvp.isPaused ⇒ <code>boolean</code>
**Kind**: instance property of [<code>GbasePvp</code>](#GbasePvp)  
**Returns**: <code>boolean</code> - - Whatever gameplay paused now  
<a name="GbasePvp+opponentPayload"></a>

### gbasePvp.opponentPayload ⇒ <code>object</code>
**Kind**: instance property of [<code>GbasePvp</code>](#GbasePvp)  
**Returns**: <code>object</code> - - Opponent's payload that was built with cloud function "pvpGeneratePayload" or provided directly by him.  
<a name="GbasePvp+startTimestamp"></a>

### gbasePvp.startTimestamp ⇒ <code>Number</code>
**Kind**: instance property of [<code>GbasePvp</code>](#GbasePvp)  
**Returns**: <code>Number</code> - - A timestamp of gameplay's start at UTF-0  
<a name="GbasePvp+randomSeed"></a>

### gbasePvp.randomSeed ⇒ <code>Number</code>
**Kind**: instance property of [<code>GbasePvp</code>](#GbasePvp)  
**Returns**: <code>Number</code> - - A random seed used to produce random values with Mersenne Twister  
<a name="GbasePvp+meIsPlayerA"></a>

### gbasePvp.meIsPlayerA ⇒ <code>boolean</code>
**Kind**: instance property of [<code>GbasePvp</code>](#GbasePvp)  
**Returns**: <code>boolean</code> - - Whatever you are player A  
<a name="GbasePvp+doConnect"></a>

### gbasePvp.doConnect(payloadObject)
Necessary action starting process of connection till "begin" event.

**Kind**: instance method of [<code>GbasePvp</code>](#GbasePvp)  

| Param | Type | Description |
| --- | --- | --- |
| payloadObject | <code>Object</code> | A payload that model will be built with. This data will get into cloud function "pvpGeneratePayload" |

<a name="GbasePvp+sendTurn"></a>

### gbasePvp.sendTurn(turnMessage)
Send turn message to your opponent. It will get into cloud function "pvpTurnHandler" if presented. The idea of turn messages
is that all they proceed in strict order with queue and directed to modify model. It is good practice to not to send turn messages
more frequently than once per second. Message should be an object

**Kind**: instance method of [<code>GbasePvp</code>](#GbasePvp)  

| Param | Type | Description |
| --- | --- | --- |
| turnMessage | <code>Object</code> | a message to send |

<a name="GbasePvp+sendDirect"></a>

### gbasePvp.sendDirect(directMessage)
Send direct message to your opponent. It will be resend to him directly without queue, modification of model or calling
any cloud function. It makes this type of messages the best way to hertz real-time gameplay, but the good practice will be
to not to send this type of messages more frequently than 15 times per second. Message should be an object

**Kind**: instance method of [<code>GbasePvp</code>](#GbasePvp)  

| Param | Type | Description |
| --- | --- | --- |
| directMessage | <code>Object</code> | a message to send |

<a name="GbasePvp+forceDestroyClient"></a>

### gbasePvp.forceDestroyClient()
Destroys pvp-client and emits "finish" event. Your opponent will be paused some time but later see "finish" event
with message of automatic game over(or dead pair)

**Kind**: instance method of [<code>GbasePvp</code>](#GbasePvp) 