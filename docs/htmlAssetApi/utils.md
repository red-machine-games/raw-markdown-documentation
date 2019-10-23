<a name="GbaseUtils"></a>

## GbaseUtils
**Kind**: global class  

* [GbaseUtils](#GbaseUtils)
    * [new GbaseUtils()](#new_GbaseUtils_new)
    * [.getServerTime(callback)](#GbaseUtils+getServerTime)
    * [.getSequence(callback)](#GbaseUtils+getSequence)
    * [.purchaseValidation()](#GbaseUtils+purchaseValidation)

<a name="new_GbaseUtils_new"></a>

### new GbaseUtils()
A section of API holding various utils

<a name="GbaseUtils+getServerTime"></a>

### gbaseUtils.getServerTime(callback)
A simple method returns local server time. Commonly it's UTC 0.

**Kind**: instance method of [<code>GbaseUtils</code>](#GbaseUtils)  

| Param | Type | Description |
| --- | --- | --- |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseUtils+getSequence"></a>

### gbaseUtils.getSequence(callback)
A simple method returns current session request sequence.

**Kind**: instance method of [<code>GbaseUtils</code>](#GbaseUtils)  

| Param | Type | Description |
| --- | --- | --- |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseUtils+purchaseValidation"></a>

### gbaseUtils.purchaseValidation()
Not tested not documented yet!

**Kind**: instance method of [<code>GbaseUtils</code>](#GbaseUtils)  