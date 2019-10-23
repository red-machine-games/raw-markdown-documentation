<a name="GbaseAccount"></a>

## GbaseAccount
**Kind**: global class  
<a name="new_GbaseAccount_new"></a>

### new GbaseAccount(fb, vk, ok, gClientId, gClientSecret, haveProfile)
A class holding all account related information. You can get it through API: GbaseAPI.currentAccount


| Param | Type | Description |
| --- | --- | --- |
| fb | <code>String</code> | Facebook ID |
| vk | <code>String</code> | VK.com ID |
| ok | <code>String</code> | OK.ru ID |
| gClientId | <code>String</code> | internal login string |
| gClientSecret | <code>String</code> | internal secret/password string |
| haveProfile | <code>boolean</code> | whether this account has any single profile |

<a name="GbaseError"></a>

## GbaseError
**Kind**: global class  
<a name="new_GbaseError_new"></a>

### new GbaseError(message, code, [details])
A class to represent standard Gbase API error


| Param | Type | Description |
| --- | --- | --- |
| message | <code>String</code> | message string |
| code | <code>Number</code> | unique code of error |
| [details] | <code>Object</code> | optional any-schema details |

<a name="GbaseRangePicker"></a>

## GbaseRangePicker
**Kind**: global class  

* [GbaseRangePicker](#GbaseRangePicker)
    * [new GbaseRangePicker([head])](#new_GbaseRangePicker_new)
    * [.range(from, to)](#GbaseRangePicker+range) ⇒ [<code>GbaseRangePicker</code>](#GbaseRangePicker)

<a name="new_GbaseRangePicker_new"></a>

### new GbaseRangePicker([head])
A chain-call range builder object for matchmaking


| Param | Type | Description |
| --- | --- | --- |
| [head] | <code>String</code> | Any range header string |

<a name="GbaseRangePicker+range"></a>

### gbaseRangePicker.range(from, to) ⇒ [<code>GbaseRangePicker</code>](#GbaseRangePicker)
Sets yet another range

**Kind**: instance method of [<code>GbaseRangePicker</code>](#GbaseRangePicker)  
**Returns**: [<code>GbaseRangePicker</code>](#GbaseRangePicker) - - this  

| Param | Type | Description |
| --- | --- | --- |
| from | <code>Number</code> | a from value |
| to | <code>Number</code> | a to value |

<a name="GbaseResponse"></a>

## GbaseResponse
**Kind**: global class  
<a name="new_GbaseResponse_new"></a>

### new GbaseResponse(isOk, [details])
Standard Gbase API response


| Param | Type | Description |
| --- | --- | --- |
| isOk | <code>boolean</code> | whether done okay |
| [details] | <code>Object</code> | optional any-schema details |