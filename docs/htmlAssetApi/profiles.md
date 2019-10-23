<a name="GbaseProfiles"></a>

## GbaseProfiles
**Kind**: global class  

* [GbaseProfiles](#GbaseProfiles)
    * [new GbaseProfiles()](#new_GbaseProfiles_new)
    * [.create(callback)](#GbaseProfiles+create)
    * [.getp(callback)](#GbaseProfiles+getp)
    * [.setp(profileData, publicProfileData, rating, mmr, wlRate, ver, callback)](#GbaseProfiles+setp)
    * [.update(profileData, publicProfileData, rating, mmr, wlRate, ver, callback)](#GbaseProfiles+update)
    * [.getPublic(targetHumanId, callback)](#GbaseProfiles+getPublic)

<a name="new_GbaseProfiles_new"></a>

### new GbaseProfiles()
A section of API host all profiles related: getting, setting and updating

<a name="GbaseProfiles+create"></a>

### gbaseProfiles.create(callback)
After first signing up, in or authentication you need a profile. You can't do anything without profile so create.
It has several root nodes: Object profileData, Object publicProfileData, int rating, int mmr, int wlRate, int ver and humanId.
You can modify them later except humanId.

**Kind**: instance method of [<code>GbaseProfiles</code>](#GbaseProfiles)  

| Param | Type | Description |
| --- | --- | --- |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseProfiles+getp"></a>

### gbaseProfiles.getp(callback)
Just get your profile after login. It's a necessary action. You DON'T need to get profile after creation.
Profile is accessible from "currentProfile" property

**Kind**: instance method of [<code>GbaseProfiles</code>](#GbaseProfiles)  

| Param | Type | Description |
| --- | --- | --- |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseProfiles+setp"></a>

### gbaseProfiles.setp(profileData, publicProfileData, rating, mmr, wlRate, ver, callback)
Fully set(rewrite) one or more root nodes. Not that I will poke you if you'll use this method more than once!

**Kind**: instance method of [<code>GbaseProfiles</code>](#GbaseProfiles)  

| Param | Type | Description |
| --- | --- | --- |
| profileData | <code>Object</code> | a schema less object representing main profile data |
| publicProfileData | <code>Object</code> | a schema less public part -  only part that available for discovery by other players |
| rating | <code>Number</code> | int used for matchmaking |
| mmr | <code>Number</code> | int used for matchmaking |
| wlRate | <code>Number</code> | int used for matchmaking |
| ver | <code>Number</code> | int representing version of particular profile. Once a while client will update and you'll need to mutate profile as far. "ver" property will help not to lost |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseProfiles+update"></a>

### gbaseProfiles.update(profileData, publicProfileData, rating, mmr, wlRate, ver, callback)
More tolerated method for keeping data in profile. "publicData" and "publicProfileData" should be presented as 1-level key-value map where
key is path(separated with dots) and value is the value. It will not fully rewrite root nodes but just change some according to these keys.
Arguments "rating", "mmr", "wlRate" and "ver" mods the same way.

**Kind**: instance method of [<code>GbaseProfiles</code>](#GbaseProfiles)  

| Param | Type | Description |
| --- | --- | --- |
| profileData | <code>Object</code> | a schema less object representing main profile data |
| publicProfileData | <code>Object</code> | a schema less public part -  only part that available for discovery by other players |
| rating | <code>Number</code> | int used for matchmaking |
| mmr | <code>Number</code> | int used for matchmaking |
| wlRate | <code>Number</code> | int used for matchmaking |
| ver | <code>Number</code> | int representing version of particular profile. Once a while client will update and you'll need to mutate profile as far. "ver" property will help not to lost |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseProfiles+getPublic"></a>

### gbaseProfiles.getPublic(targetHumanId, callback)
Just get someone's publicProfileData

**Kind**: instance method of [<code>GbaseProfiles</code>](#GbaseProfiles)  

| Param | Type | Description |
| --- | --- | --- |
| targetHumanId | <code>Number</code> | self titled |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |