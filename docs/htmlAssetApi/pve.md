<a name="GbasePve"></a>

## GbasePve
**Kind**: global class  

* [GbasePve](#GbasePve)
    * [new GbasePve()](#new_GbasePve_new)
    * [.begin(beginParams, callback)](#GbasePve+begin)
    * [.act(actParams, callback)](#GbasePve+act)
    * [.listBattles(skip, limit, callback)](#GbasePve+listBattles)

<a name="new_GbasePve_new"></a>

### new GbasePve()
A section of API holding simplified PvE mechanism

<a name="GbasePve+begin"></a>

### gbasePve.begin(beginParams, callback)
Pve is a set of acts programmed with 3 cloud functions: "pveInit", "pveAct" and "pveFinalize". Fist to are called directly
from client and the last one is called automatically by decision of "pveAct" cloud function. Possible to pass any params
to "pveInit" cloud function.

**Kind**: instance method of [<code>GbasePve</code>](#GbasePve)  

| Param | Type | Description |
| --- | --- | --- |
| beginParams | <code>Object</code> | any plain object will be accessed on pveInit |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbasePve+act"></a>

### gbasePve.act(actParams, callback)
An action or turn of PvE cycle. Backed with "pveAct" cloud function which can finalize pve cycle and call "pveFinalize"
cloud function. The same way some params can be passed.

**Kind**: instance method of [<code>GbasePve</code>](#GbasePve)  

| Param | Type | Description |
| --- | --- | --- |
| actParams | <code>Object</code> | any plain object will be accessed on pveAct |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbasePve+listBattles"></a>

### gbasePve.listBattles(skip, limit, callback)
Pve cycle will produce battle journal entry. It's made by calling function "appendSelfBattleJournalPve"
from cloud function "pveFinalize". It'll contain some details on battle plus schema-less data from cloud code programmer.

**Kind**: instance method of [<code>GbasePve</code>](#GbasePve)  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| skip | <code>Number</code> | <code>0</code> | pagination skip |
| limit | <code>Number</code> | <code>20</code> | pagination limit |
| callback | <code>function</code> |  | standard callback(GbaseError err, GbaseResponse response) |