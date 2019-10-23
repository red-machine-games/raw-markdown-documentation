# Cloud Functions programming reference

## Global variables

| Global variable | Type | Description | Schema |
|-------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| `#!js resources` | Object | A set of data that was configured in cluster configuration file. It can be just a json or external path to json. | any |
| `#!js session` | Object | An object to store some operative data for particular player. It's linked with actual operative session and will renew with it. Use it as operative cache or else. | any |
| `#!js glob` | Object | An object to globaly store any data set at `#!js initContext.js` cloud function. | any |
| `#!js runtimeVersions` | Object | System information about environment versions. | `#!js app` : _String_ - Goblin Backend version |
|  |  |  | `#!js node` : _String_ - Node.js version |
|  |  |  | `#!js v8` : _String_ - Node's V8 engine version |
|  |  |  | `#!js lodash` : _String_ - Lodash version |
|  |  |  | `#!js env` : _String_ - environment flag |
| `#!js selfHumanId` | Number | Caller Human ID. Can be `#!js null`. |  |
| `#!js now` | Number | Call timestamp in milliseconds. |  |
| `#!js args` | Object | A dictionary with input arguments. | any |
| `#!js clientParams` | Object | An object with data sent by client caller. | any |

## Service functions

| Global variable | Type | Description | Arguments | Return | Return schema |
|------------------------|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|---------|---------------------------------------------------------------------------|
| `#!js defineGlobal` | Function | A function to define property in `#!js glog` object that would be available from all cloud functions. Only can be called from `#!js initContext.js` cloud function. | `#!js variableName` - _String_ - self-titled |  |  |
|  |  |  | `#!js theValue` - _any_ - self-titled |  |  |
| `#!js validateStoreReceipt` | async Function | To validate App Store or Google Play receipt. | `#!js receiptBody` : _String_/_Object_ - Self-titled | Object | `#!js isValid` : _boolean_ - whether receipt is valid |
|  |  |  | `#!js head` : _String_ - A header of store credentials. Mostly keep it `#!js null` to use default credentials but it's possible to diversify validation credentials |  | `#!js butDuplicated` : _boolean_ - whether it was already validated previously |
| `#!js lock.self` | async Function | Get the lock on your self. |  |  |  |
| `#!js lock.selfAnd` | async Function | Get the lock on your self and somebody else. | `#!js arrayOfHids` : _Array_ - who else to lock |  |  |
| `#!js lock.some` | async Function | Get the lock on somebody else. | `#!js arrayOfHids` : _Array_ - who to lock |  |  |
| `#!js lock.check` | Function | Get array of locked Human IDs. |  | Array |  |
| `#!js relock.self` | async Function | Returns any current locks and then do `#!js lock.self`. |  |  |  |
| `#!js relock.selfAnd` | async Function | Returns any current locks and then do `#!js lock.selfAnd`. | `#!js arrayOfHids` : _Array_ - who else to lock |  |  |
| `#!js relock.some` | async Function | Returns any current locks and then do `#!js lock.some`. | `#!js arrayOfHids` : _Array_ - who to lock |  |  |
| `#!js checkIsBot` | async Function | Checks by provided human ID that it belongs to bot profile. | `#!js targetHumanId` : _Number_ - who to check | boolean |  |
| `#!js run` | async Function | Runs another cloud function. It should be allowed to run internally | `#!js functionName` : _String_ - self-titled | Object | any |
|  |  |  | `#!js functionArguments` : _Object_ - whatever to provide into calling function |  |  |
| `#!js access.onlyInternal` | Function | Throws an error if called by route. |  |  |  |
| `#!js access.onlyExternal` | Function | Throws an error if called from another cloud function. |  |  |  |
| `#!js setTimeout` | async Function | A standard timeout with Promise but total wait time is limited(mostly with 10 secs). |  |  |  |  

## Business logic functions

| Global variable | Type | Description | Arguments | Return | Return schema |
|-----------------------------------------------|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|-------------------------------------------------------|
| `#!js getProfileNode` | async Function | Gets the pointed node of `#!js profileData` root node of cloud function caller. | `#!js nodePath` : _String_ - a json-path what to get | any |  |
|  |  |  | `#!js skip` : _Number_ - skip in case of getting values of array |  |  |
|  |  |  | `#!js limit` : _Number_ - limit in case of getting values of array |  |  |
| `#!js getSomeProfileNode` | async Function | Gets the pointed node of `#!js profileData` root node of target player. | `#!js targetHumanId` : _Number_ - target player's Human ID | any |  |
|  |  |  | `#!js nodePath` : _String_ - a json-path what to get |  |  |
|  |  |  | `#!js skip` : _Number_ - skip in case of getting values of array |  |  |
|  |  |  | `#!js limit` : _Number_ - limit in case of getting values of array |  |  |
| `#!js getPublicProfileNode` | async Function | Gets the pointed node of `#!js publicProfileData` root node of target player. | `#!js targetHumanId` : _Number_ - target player's Human ID | any |  |
|  |  |  | `#!js nodePath` : _String_ - a json-path what to get |  |  |
|  |  |  | `#!js skip` : _Number_ - skip in case of getting values of array |  |  |
|  |  |  | `#!js limit` : _Number_ - limit in case of getting values of array |  |  |
| `#!js setProfileNode` | Function | Sets the node of cloud function caller. It can be: `#!js wlRate`, `#!js ver`, `#!js mmr`, `#!js profileData`, `#!js publicProfileData` or their subnodes. This updating is not at time i.e. update will take effect after this particular function call ends. You can't set conflicting nodes - nodes with same path but one of them deeper. | `#!js nodePath` : _String_ - json-path to node |  |  |
|  |  |  | `#!js value` : _any_ - any value to set. Can't be `#!js NaN` |  |  |
| `#!js setSomeProfileNode` | Function | Sets the node of target player. It can be: `#!js wlRate`, `#!js ver`, `#!js mmr`, `#!js profileData`, `#!js publicProfileData` or their subnodes. This updating is not at time i.e. update will take effect after this particular function call ends. You can't set conflicting nodes - nodes with same path but one of them deeper. | `#!js targetHumanId` : _Number_ - target player's Human ID | any |  |
|  |  |  | `#!js nodePath` : _String_ - json-path to node |  |  |
|  |  |  | `#!js value` : _any_ - any value to set. Can't be `#!js NaN` |  |  |
| `#!js getSelfRating` | async Function | Gets cloud function caller's rating value from particular segment. | `#!js segment` : _String_ - self-titled | Number |  |
| `#!js setSelfRating` | Function | Sets cloud function caller's rating into particular segment. | `#!js segment` : _String_ - self-titled |  |  |
|  |  |  | `#!js value` : _Number_ - self-titled |  |  |
| `#!js getSomeonesRating` | async Function | Gets someone's rating value from particular segment. | `#!js targetHumanId` : _Number_ - target player's Human ID | Number |  |
|  |  |  | `#!js segment` : _String_ - self-titled |  |  |
| `#!js setSomeonesRating` | Function | Sets someone's rating into particular segment. | `#!js segment` : _String_ - self-titled |  |  |
|  |  |  | `#!js value` : _Number_ - self-titled |  |  |
| `#!js getSelfRatings` | async Function | Gets all cloud function caller's records from all segments as key-value object. |  | Object | `#!js { segment: value, ... }` |
| `#!js checkForBattleDebts` | async Function | A function to check current PvE state of player - the player can play only one PvE game at moment. Function checks whether player currently at PvE, if true - returns positive response and deletes game from operative memory, you can hang sanctions on him. |  | boolean |  |
| `#!js matchmaking .getPlayer` | async Function | Match a player without continuation. Use it to search players by rating value in some particular segment. | `#!js segment` : _String_ - target segment | Object | `#!js humanId` : _Number_ - a Human ID of opponent |
|  |  |  | `#!js ranges` : _Array_ - array of objects with strict schema like `#!js [{ from: 1, to: 2 }, { from: 3, to: 4 }]` representing ranges of search among rating values. Also use values `#!js -inf` and `#!js +inf` to represent negative or positive infinity |  |  |
|  |  |  | `#!js nRandom` : _Number_ - optional argument gives ability to pick the random opponent among some amount of appropriate candidates. Gives you randomness |  |  |
|  |  |  | `#!js rememberMatchForMs` : _Number_ - optional argument means that picked opponent will be unavailable to match for some time in milliseconds or till the next matchmake |  |  |
| `#!js matchmaking .getPlayerByLadder` | async Function | Match a player without continuation. Use it to search players by place in rating ladder in some particular segment. | `#!js segment` : _String_ - target segment | Object | `#!js humanId` : _Number_ - a Human ID of opponent |
|  |  |  | `#!js ranges` : _Array_ - array of objects with strict schema like `#!js [{ from: 1, to: 2 }, { from: 3, to: 4 }]` representing ranges of search among places in rating ladder. Also use values `#!js -inf` and `#!js +inf` to represent negative or positive infinity |  |  |
|  |  |  | `#!js nRandom` : _Number_ - optional argument gives ability to pick the random opponent among some amount of appropriate candidates. Gives you randomness |  |  |
|  |  |  | `#!js rememberMatchForMs` : _Number_ - optional argument means that picked opponent will be unavailable to match for some time in milliseconds or till the next matchmake |  |  |
| `#!js matchmaking .getBot` | async Function | Search among bot profiles by bot's rating value. Bot ratings are in separate nameless segment. | `#!js ranges` : _Array_ - array of objects with strict schema like `#!js [{ from: 1, to: 2 }, { from: 3, to: 4 }]` representing ranges of search among rating values. Also use values `#!js -inf` and `#!js +inf` to represent negative or positive infinity | Object | `#!js botHumanId` : _Number_ - a Human ID of bot opponent |
|  |  |  | `#!js nRandom` : _Number_ - optional argument gives ability to pick the random opponent among some amount of appropriate candidates. Gives you randomness |  |  |
|  |  |  | `#!js rememberMatchForMs` : _Number_ - optional argument means that picked opponent will be unavailable to match for some time in milliseconds or till the next matchmake |  |  |
| `#!js matchmaking .checkPvpNoSearch` | async Function | Get cloud function caller's current PvP state. If it in - pvp room data and booking key returns. |  | Object | `#!js c`: _Number_ - connection status from -1 |
|  |  |  |  |  | `#!js stat` : _String_ - string representation of status |
|  |  |  |  |  | `#!js address` : _Object_ - optional pvp room address data |
|  |  |  |  |  | `#!js key` : _String_ - optional booking key |
| `#!js matchmaking .dropMatchmaking` | async Function | Useful service method to force clean all matchmaking data. |  | Object | `#!js forReal` : _boolean_ - was it dropped for real |
| `#!js matchmaking .searchPvpOpponent` | async Function | Matchmaking pvp opponent in real time by rating value at particular segment. Blocks awaiting for some time and can return object like either `#!js { stat: 'MM: timeout', c: -1 }` or `#!js { stat: 'MM: searching', c: 0 }` or `#!js { stat: 'MM: accept or decline the game', c: 1 }` or `#!js { stat: 'MM: waiting for opponent to accept the game', c: 2 }` or `#!js { stat: 'MM: gameroom allocated', c: 3, address: { pvp room address data }, key: 'a booking key' }`. In case of `#!js c === -1` - search is timed out, you can repeat or just respond player to try later. In case of `#!js c === 0` - just repeat call. | `#!js segment` : _String_ - target segment | Object | `#!js c`: _Number_ - connection status from -1 |
|  |  |  | `#!js ranges` : _Array_ - array of objects with strict schema like `#!js [{ from: 1, to: 2 }, { from: 3, to: 4 }]` representing ranges of search among rating values. Also use values `#!js -inf` and `#!js +inf` to represent negative or positive infinity |  | `#!js stat` : _String_ - string representation of status |
|  |  |  | `#!js nRandom` : _Number_ - optional argument gives ability to pick the random opponent among some amount of appropriate candidates. Gives you randomness |  | `#!js address` : _Object_ - optional pvp room address data |
|  |  |  |  |  | `#!js key` : _String_ - optional booking key |
| `#!js matchmaking .searchPvpOpponentByLadder` | async Function | Matchmaking pvp opponent in real time by rating ladder place in particular segment. Blocks awaiting for some time and can return object like either `#!js { stat: 'MM: timeout', c: -1 }` or `#!js { stat: 'MM: searching', c: 0 }` or `#!js { stat: 'MM: accept or decline the game', c: 1 }` or `#!js { stat: 'MM: waiting for opponent to accept the game', c: 2 }` or `#!js { stat: 'MM: gameroom allocated', c: 3, address: { pvp room address data }, key: 'a booking key' }`. In case of `#!js c === -1` - search is timed out, you can repeat or just respond player to try later. In case of `#!js c === 0` - just repeat call. | `#!js segment` : _String_ - target segment | Object | `#!js c`: _Number_ - connection status from -1 |
|  |  |  | `#!js ranges` : _Array_ - array of objects with strict schema like `#!js [{ from: 1, to: 2 }, { from: 3, to: 4 }]` representing ranges of search among places in rating ladder. Also use values `#!js -inf` and `#!js +inf` to represent negative or positive infinity |  | `#!js stat` : _String_ - string representation of status |
|  |  |  | `#!js nRandom` : _Number_ - optional argument gives ability to pick the random opponent among some amount of appropriate candidates. Gives you randomness |  | `#!js address` : _Object_ - optional pvp room address data |
|  |  |  |  |  | `#!js key` : _String_ - optional booking key |
| `#!js matchmaking .searchPvpBotOpponent` | async Function | Pick bot profile to play PvP versus self. Can return either `#!js { stat: 'MM: accept or decline the game', c: 1 }` or `#!js null` in case that no appropriate bot profile. | `#!js ranges` : _Array_ - array of objects with strict schema like `#!js [{ from: 1, to: 2 }, { from: 3, to: 4 }]` representing ranges of search among rating values. Also use values `#!js -inf` and `#!js +inf` to represent negative or positive infinity | Object | `#!js c`: _Number_ - connection status from -1 |
|  |  |  | `#!js nRandom` : _Number_ - optional argument gives ability to pick the random opponent among some amount of appropriate candidates. Gives you randomness |  | `#!js stat` : _String_ - string representation of status |
| `#!js matchmaking .stopSearchingForAnyPvpOpponent` | async Function | Stop searching for real opponent. Works only if your matchmaking status is `#!js MM: searching` - respond with `#!js { stat: 'MM: no more waiting', c: -1 }`. Otherwise it can respond with either `#!js { stat: 'MM: accept or decline the game', c: 1 }` or `#!js { stat: 'MM: waiting for opponent to accept the game', c: 2 }`. |  | Object | `#!js c`: _Number_ - connection status from -1 |
|  |  |  |  |  | `#!js stat` : _String_ - string representation of status |
| `#!js matchmaking .handSelectPvpOpponent` | async Function | Start a PvP versus self pointing fictive opponent's Human ID. Appropriate method to play asynchronous multiplayer. Can return either object `#!js { stat: 'MM: accept or decline the game', c: 1 }` or `#!js null` if no player found with provided Human ID. | `#!js targetHumanId` : _Number_ - fictive opponent's Human ID | Object | `#!js c`: _Number_ - connection status from -1 |
|  |  |  |  |  | `#!js stat` : _String_ - string representation of status |
| `#!js matchmaking .acceptPvpMatch` | async Function | Accepts PvP matchmaking at state `#!js { stat: 'MM: accept or decline the game', c: 1 }`. Can return object like either `#!js { stat: 'MM: waiting for opponent to accept the game', c: 2 }` or `#!js { stat: 'MM: gameroom allocated', c: 3, address: { pvp room address data }, key: 'a booking key' }`. |  | Object | `#!js c`: _Number_ - connection status from -1 |
|  |  |  |  |  | `#!js stat` : _String_ - string representation of status |
|  |  |  |  |  | `#!js address` : _Object_ - optional pvp room address data |
|  |  |  |  |  | `#!js key` : _String_ - optional booking key |
| `#!js matchmaking .waitForPvpOpponentToAccept` | async Function | A blocking async function that waits for real opponent to call `#!js acceptPvpMatch` or `#!js declinePvpMatch` on his side. Returns object like either `#!js { stat: 'MM: waiting for opponent to accept the game', c: 2 }` - in this case you just should repeat call, or `#!js { stat: 'MM: timeout', c: -1 }` - in this case you can't do anything due to matchmaking timed out and dead, or `#!js { stat: 'MM: gameroom allocated', c: 3, address: { pvp room address data }, key: 'a booking key' }`. |  | Object | `#!js c`: _Number_ - connection status from -1 |
|  |  |  |  |  | `#!js stat` : _String_ - string representation of status |
|  |  |  |  |  | `#!js address` : _Object_ - optional pvp room address data |
|  |  |  |  |  | `#!js key` : _String_ - optional booking key |
| `#!js matchmaking .declinePvpMatch` | async Function | Declines PvP matchmaking at state `#!js { stat: 'MM: accept or decline the game', c: 1 }`. Can return object like either `#!js { stat: 'MM: match declined', c: -1 }` or `#!js { stat: 'MM: waiting for opponent to accept the game', c: 2 }` or `#!js { stat: 'MM: gameroom allocated', c: 3, address: { pvp room address data }, key: 'a booking key' }`. |  | Object | `#!js c`: _Number_ - connection status from -1 |
|  |  |  |  |  | `#!js stat` : _String_ - string representation of status |
|  |  |  |  |  | `#!js address` : _Object_ - optional pvp room address data |
|  |  |  |  |  | `#!js key` : _String_ - optional booking key |
| `#!js appendBattleJournalPve` | Function | Puts an entry into PvE battle journal for some target player. It can contain any data. Atomic towards profile modifications, records, receipt validations and other battle journal entries. | `#!js targetHumanId` : _Number_ - self-titled |  |  |
|  |  |  | `#!js theData` : _Object_ - any senseful data that should be added into entry |  |  |
| `#!js appendSelfBattleJournalPve` | Function | Puts an entry into PvE battle journal for cloud function caller. It can contain any data. Atomic towards profile modifications, records, receipt validations and other battle journal entries. | `#!js theData` : _Object_ - any senseful data that should be added into entry |  |  |
| `#!js appendBattleJournalPvp` | Function | Puts an entry into PvP battle journal for actual opponents. It can contain any data payload along with data about both opponents. Atomic towards profile modifications, records, receipt validations and other battle journal entries. | `#!js theData` : _Object_ - any senseful data that should be added into entry |  |  |
|  |  |  | `#!js isAuto` : _boolean_ - whether entry made due to automatic game over |  |  |
|  |  |  | `#!js doNotPersistOpponent` : _boolean_ - flag whether to include data about opponent B into entry. Useful if you play versus self as player A with fictive opponent as player B |  |  |

## Response functions

| Global variable | Type | Description | Arguments | Argument schema | Call as error |
|----------------------------|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|--------------------------------------|
| `#!js CreateNewProfileResponse` | Function | Response function used at the end of `#!js createNewProfile.js` cloud function. | `#!js profileBody` : _Object_ - a profile body to make new profile from. Should contain strict set of root nodes | `#!js profileData` : _Object_ - any-schema node of private profile data |  |
|  |  |  |  | `#!js publicProfileData` : _Object_ - any-schema node of public profile data |  |
|  |  |  |  | `#!js ver` : _Number_ - a version number of particular profile body. 1 by default |  |
|  |  |  |  | `#!js mmr` : _Number_ - a matchmaking rating of profile |  |
|  |  |  |  | `#!js wlRate` : _Number_ - a win-lose rate of profile |  |
| `#!js MutateProfileResponse` | Function | Response function used at the end of `#!js mutateProfile.js` cloud function. | `#!js silentError` : _String_ - an error message that should go to player during any try to mutate profile if called method `#!js asError` of function's response |  | `#!js MutateProfileResponse() .asError()` |
| `#!js FunctionResponse` | Function | A standard response for any custom cloud function. | `#!js objectToReturn` : _any_ - represents any information that particular cloud function returns. Furthermore can be an error message if called method `#!js asError` of function's response. The data returns to caller or to another cloud function that called this one |  | `#!js FunctionResponse() .asError()` |
| `#!js PveInitResponse` | Function | Response function used at the end of `#!js pveInit.js` cloud function. | `#!js theModel` : _Object_ - operative model to play with made during initialization process |  | `#!js PveInitResponse() .asError()` |
|  |  |  | `#!js objectToReturn` : _any_ - represents any information that goes back to function caller. Furthermore can be an error message if called method `#!js asError` of function's response |  |  |
| `#!js PveActResponse` | Function | Response function used at the end of `#!js pveAct.js` cloud function. | `#!js gameIsOver` : _boolean_ - whether game should be considered as over and `#!js pveFinalize.js` cloud function be called next |  | `#!js PveActResponse() .asError()` |
|  |  |  | `#!js battleModel` : _Object_ - optional modified battle model to push into operative memory. If not provided - not updated |  |  |
|  |  |  | `#!js objectToReturn` : _any_ - represents any information that goes back to function caller. Furthermore can be an error message if called method `#!js asError` of function's response |  |  |
| `#!js PveFinalizeResponse` | Function | Response function used at the end of `#!js pveFinalize.js` cloud function. |  |  |  |
| `#!js PvpResponse` | Function | Response function used at the end of `#!js pvpGeneratePayload.js`, `#!js pvpInitGameplayModel.js`, `#!js pvpGameOverHandler.js` and `#!js pvpCheckGameOver.js` cloud functions. | `#!js objectToReturn` : _any_ - represents any information that goes back to pvp mechanism |  |  |
| `#!js PvpMessageHandler` | Function | Response function used at the end of `#!js pvpTurnHandler.js` cloud function. | `#!js modifiedModel` : _Object_ - optional modified battle model to push into operative memory. If not provided - not updated |  |  |
|  |  |  | `#!js messageForOpponentA` : _Object_ - optional message that goes to player A |  |  |
|  |  |  | `#!js messageForOpponentB` : _Object_ - optional message that goes to player B |  |  |
| `#!js PvpConnectionHandler` | Function | Response function used at the end of `#!js pvpConnectionHandler.js` cloud function. | `#!js messageForConnectedPlayer` : _Object_ - self-titled |  |  |
|  |  |  | `#!js messageForOpponentPlayer` : _Object_ - self-titled |  |  |
| `#!js PvpDisconnectionHandler` | Function | Response function used at the end of `#!js pvpDisconnectionHandler.js` cloud function. | `#!js messageForConnectedOpponent` : _Object_ - self-titled |  |  |
| `#!js PvpAutoDefeatResponse` | Function | Response function used at the end of `#!js pvpAutoCloseHandler.js` cloud function. | `#!js messageForOpponentA` : _Object_ - optional message that goes to player A |  |  |
|  |  |  | `#!js messageForOpponentB` : _Object_ - optional message that goes to player B |  |  |
| `#!js OnMatchmakingResponse` | Function | Response function used at the end of `#!js pvpAutoCloseHandler.js` cloud function. | `#!js segment` : _String_ - in which segment to matchmake |  | `#!js OnMatchmakingResponse() .asError()` |
|  |  |  | `#!js strategy` : _String_ - how to matchmake: either `#!js byr` or `#!js bylad` |  |  |
|  |  |  | `#!js mmDetails` : _Object_ - object with search `rgs` and `nran` | `#!js rgs`: _Array_ - array of objects with strict schema like `#!js [{ from: 1, to: 2 }, { from: 3, to: 4 }]` representing ranges of search among rating values. Also use values `#!js -inf` and `#!js +inf` to represent negative or positive infinity |  |
|  |  |  |  | `#!js nran` : _Number_ - optional argument gives ability to pick the random opponent among some amount of appropriate candidates. Gives you randomness |  |