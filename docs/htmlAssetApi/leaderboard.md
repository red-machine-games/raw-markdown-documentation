<a name="GbaseLeaderboards"></a>

## GbaseLeaderboards
**Kind**: global class  

* [GbaseLeaderboards](#GbaseLeaderboards)
    * [new GbaseLeaderboards()](#new_GbaseLeaderboards_new)
    * [.postRecord(value, intoSegment, callback)](#GbaseLeaderboards+postRecord)
    * [.getSelfRecord(fromSegment, callback)](#GbaseLeaderboards+getSelfRecord)
    * [.getLeaderboardOverall(fromSegment, skip, limit, callback)](#GbaseLeaderboards+getLeaderboardOverall)
    * [.removeSelfRecord(fromSegment, callback)](#GbaseLeaderboards+removeSelfRecord)
    * [.refreshVkFriendsCache(friendsArray, callback)](#GbaseLeaderboards+refreshVkFriendsCache)
    * [.refreshOkFriendsCache(friendsArray, callback)](#GbaseLeaderboards+refreshOkFriendsCache)
    * [.refreshFbFriendsCache(friendsArray, callback)](#GbaseLeaderboards+refreshFbFriendsCache)
    * [.getSomeonesRating(targetHumanId, fromSegment, callback)](#GbaseLeaderboards+getSomeonesRating)
    * [.getLeadersWithinFriends(fromSegment, skip, limit, callback)](#GbaseLeaderboards+getLeadersWithinFriends)

<a name="new_GbaseLeaderboards_new"></a>

### new GbaseLeaderboards()
A section of API host all leaderboards/records related: getting, setting and updating

<a name="GbaseLeaderboards+postRecord"></a>

### gbaseLeaderboards.postRecord(value, intoSegment, callback)
Post a record into some segment(segments are white listed at Gbase configurations). All new posts in same segment will overwrite each other.

**Kind**: instance method of [<code>GbaseLeaderboards</code>](#GbaseLeaderboards)  

| Param | Type | Description |
| --- | --- | --- |
| value | <code>Number</code> | positive int |
| intoSegment | <code>String</code> | string, default="def" |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseLeaderboards+getSelfRecord"></a>

### gbaseLeaderboards.getSelfRecord(fromSegment, callback)
Get self record from some segment. Self titled

**Kind**: instance method of [<code>GbaseLeaderboards</code>](#GbaseLeaderboards)  

| Param | Type | Description |
| --- | --- | --- |
| fromSegment | <code>String</code> | string, default="def" |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseLeaderboards+getLeaderboardOverall"></a>

### gbaseLeaderboards.getLeaderboardOverall(fromSegment, skip, limit, callback)
List leaders from some segment. Use arguments "skip" and "limit"(maximum 20) for pagination.

**Kind**: instance method of [<code>GbaseLeaderboards</code>](#GbaseLeaderboards)  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| fromSegment | <code>String</code> |  | string, default="def" |
| skip | <code>Number</code> | <code>0</code> | int, min 0 |
| limit | <code>Number</code> | <code>20</code> | int, min 1 max 20 |
| callback | <code>function</code> |  | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseLeaderboards+removeSelfRecord"></a>

### gbaseLeaderboards.removeSelfRecord(fromSegment, callback)
Remove player's record from some segment

**Kind**: instance method of [<code>GbaseLeaderboards</code>](#GbaseLeaderboards)  

| Param | Type | Description |
| --- | --- | --- |
| fromSegment | <code>String</code> | string, default="def" |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseLeaderboards+refreshVkFriendsCache"></a>

### gbaseLeaderboards.refreshVkFriendsCache(friendsArray, callback)
Before taking leaders among social friends you need to tell Goblin Backend about your friends. It does't use any social SDK to
get your friends by itself hence it's on your side.

**Kind**: instance method of [<code>GbaseLeaderboards</code>](#GbaseLeaderboards)  

| Param | Type | Description |
| --- | --- | --- |
| friendsArray | <code>Array</code> | just an array of your friends' IDs |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseLeaderboards+refreshOkFriendsCache"></a>

### gbaseLeaderboards.refreshOkFriendsCache(friendsArray, callback)
Before taking leaders among social friends you need to tell Goblin Backend about your friends. It does't use any social SDK to
get your friends by itself hence it's on your side.

**Kind**: instance method of [<code>GbaseLeaderboards</code>](#GbaseLeaderboards)  

| Param | Type | Description |
| --- | --- | --- |
| friendsArray | <code>Array</code> | just an array of your friends' IDs |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseLeaderboards+refreshFbFriendsCache"></a>

### gbaseLeaderboards.refreshFbFriendsCache(friendsArray, callback)
Before taking leaders among social friends you need to tell Goblin Backend about your friends. It does't use any social SDK to
get your friends by itself hence it's on your side.

**Kind**: instance method of [<code>GbaseLeaderboards</code>](#GbaseLeaderboards)  

| Param | Type | Description |
| --- | --- | --- |
| friendsArray | <code>Array</code> | just an array of your friends' IDs |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseLeaderboards+getSomeonesRating"></a>

### gbaseLeaderboards.getSomeonesRating(targetHumanId, fromSegment, callback)
To get someone's rating. Just a simple method to retrieve rating by providing existing human-ID.

**Kind**: instance method of [<code>GbaseLeaderboards</code>](#GbaseLeaderboards)  

| Param | Type | Description |
| --- | --- | --- |
| targetHumanId | <code>Number</code> | self titled |
| fromSegment | <code>String</code> | self titled |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseLeaderboards+getLeadersWithinFriends"></a>

### gbaseLeaderboards.getLeadersWithinFriends(fromSegment, skip, limit, callback)
A special method to get leaders only among your social friends. It'll determine your social platform if you logged before.
A very need to provide list of your friends first using methods "refresh*FriendsCache". If no friends on Goblin Backend you'll get
just positive response without any data. A friends cache lives approximately for 1 week(configured on backend). So just try to
refresh your friends once a while but not too often.

**Kind**: instance method of [<code>GbaseLeaderboards</code>](#GbaseLeaderboards)  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| fromSegment | <code>String</code> |  | self titled |
| skip | <code>Number</code> | <code>0</code> | pagination skip (from 0 to infinity) |
| limit | <code>Number</code> | <code>20</code> | pagination limit (from 1 to 20) |
| callback | <code>function</code> |  | standard callback(GbaseError err, GbaseResponse response) |