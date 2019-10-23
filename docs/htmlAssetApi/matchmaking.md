<a name="GbaseMatchmaking"></a>

## GbaseMatchmaking
**Kind**: global class  

* [GbaseMatchmaking](#GbaseMatchmaking)
    * [new GbaseMatchmaking()](#new_GbaseMatchmaking_new)
    * [.matchPlayer(fromSegment, strategy, ranges, callback)](#GbaseMatchmaking+matchPlayer)
    * [.matchBot()](#GbaseMatchmaking+matchBot)

<a name="new_GbaseMatchmaking_new"></a>

### new GbaseMatchmaking()
A section of API holding matchmaking as a simple search without actual play

<a name="GbaseMatchmaking+matchPlayer"></a>

### gbaseMatchmaking.matchPlayer(fromSegment, strategy, ranges, callback)
Matchmaking - is a mechanism to pick some player to do the work with. Mostly to play with but you can interpret it as a search.
Searching done by rating, picking the appropriate opponent by some rating value from some segment. All the data is up to you.
In return you will get the Human-ID

**Kind**: instance method of [<code>GbaseMatchmaking</code>](#GbaseMatchmaking)  

| Param | Type | Description |
| --- | --- | --- |
| fromSegment | <code>String</code> | self titled |
| strategy | <code>String</code> | if backend is configured to receive client-defined strategy, you can pick one from two available: GbaseApi.MATCHMAKING_STRATEGIES.BY_RATING and GbaseApi.MATCHMAKING_STRATEGIES.BY_LADDER |
| ranges | <code>GbaseRangePicker</code> | an instance with ranges of search. See test examples |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseMatchmaking+matchBot"></a>

### gbaseMatchmaking.matchBot()
Not tested not documented yet

**Kind**: instance method of [<code>GbaseMatchmaking</code>](#GbaseMatchmaking)  