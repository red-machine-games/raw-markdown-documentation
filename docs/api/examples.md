# Specifications and how-tos ⛈️

## Any developer-defined cloud function

==The idea of cloud functions is to bring the power to incapsulate any logic into cloud==. You can chase several goals using cloud functions, separately or in conjunction:

- Simply make domain logic authoritarian to prevent unfair logic substitution. The big community now has many tools to make it real, especially if you deployed on web stack;
- To provide atomicity for sets of reads and writes. If you develop something more complicated than school calculator your case will look like this: you read something, you doing some check and logic on read data, and then you write, and your real case may be far bigger. All reads and writes corresponds to separate http-requests, the one for each step and you don't need to be a rocket scientist to guess that no atomicity here. Cloud functions can give you that read-check-whatever-write-etc atomicity due to employed _ACID engine_;
- To validate store receipts. Even the little app will face the people that fakes receipts. It's easy to cheat on client by faking store backend response and making client to communicate with it. Backend-side validation is unreachable for this trick and also done with _ACID engine_;
- To rid client of heavy calculations.

Back to code. First of all you should define whether this cloud function will be called externally or internally, or both. Place `#!js access.onlyExternal();` or `#!js access.onlyInternal();` on top of code. You don't need to place it on predefined cloud function because they are unreachable in both ways. Then let's check 2 important variables: `#!js clientParams` and `#!js args`. `#!js clientParams` has data from sender(sended through network protocol or put into `#!js await run('cloudFunctionName', clientParams)`). `#!js args` has useful domain args provided by Goblin Backend platform. By default you'll find: `#!js clientPlatform` and `#!js clientVersion`(provided by client on GbaseApi initialization). Then we need to get lock for player(s) to read and/or write: `#!js await lock.self()` or analogue. at the end we should call appropriate response function(e.g. `#!js FunctionResponse`) with `#!js return` statement to stop the execution.

!!! warning "Some general hints very desirable for implementation"
    - **Try to minimise database workload!** Call reads and writes as rare as possible, also retrieve as little as possible data, organize it wisely to read and write all needed by one call. Furthermore think about total size of profile and strictly try to not to put useless data there - for example some general data that could store at Balance.json static file, totally useless or deprecated data;
    - Use `#!js session` global variable to cache highly frequent data into operative memory;
    - Do not relock without strict reason and do not lock useless human IDs;
    - Use log only for debugging, remove these lines as soon as no longer needed;
    - Do not use `#!js setTimeout`;
    - If you need to develop some gameplay timers - just use timestamps: use global variable `#!js now`(represents timestamp of cloud function call with milliseconds), persist, read and compare it;
    - use `#!js ===` instead of `#!js ==`;
    - Don't forget that all writes into database are made ==**AFTER**== execution;
    - Share responsibilities with internal run only cloud functions and `#!js run` API;
    - Validate all values whether they not `#!js NaN` before persisting 'em.

## Developing `createNewProfile.js` cloud function

This one should be as simple as possible, just to represent actual domain profile schema by default. Don't forget to update it and use static files like `Balance.json` as data source: like `CreateNewProfileResponse({ profileData: { someData: resources.Balance.someDataSource } });`.

## Developing `mutateProfile.js` cloud function

Here you should implement ==mutation chain== method. When you have own domain schema for profiles it most likely to update along with client itself, so here are 2 things to implement chain of mutation: **profile version** and **mutateProfile.js cloud function**.

!!! example "Here is algorithm"
    1. Get profile's version;
    2. Add new mutation functions on demand;
    3. Each function must compare profile's version and desirable, make updated of desirable more than actual;
    4. Call the next mutation function in a row;
    5. Set version to the most actual after chain.

_Here's example of mutations chain_:
```javascript
const ACTUAL_PROFILE_VER = 4;

await lock.self();

var ver = await getProfileNode('ver');

async function mutateV1(){
	if(ver < 2){
		// Do updates on profile #1
	}
	await mutateV2();
}
async function mutateV2(){
	if(ver < 3){
		// Do updates on profile #2
	}
	await mutateV3();
}
async function mutateV3(){
	if(ver < 4){
		// Do updates on profile #3
	}
	endMutation();
}
function endMutation(){
	setProfileNode('ver', ACTUAL_PROFILE_VER);

	MutateProfileResponse();
}

if(ver < ACTUAL_PROFILE_VER){   // Let's skip mutation if we surely know that profile is fully actual
    await mutateV1();
} else {
    MutateProfileResponse();
}
```
{>>And as you can see 2 things should be done along time: appending new mutation functions and incrementing end version of profile<<}

## Developing `onGetProfile.js` cloud function

Use it for small service profile changes. Differs from `mutateProfile.js` in that it called after mutation and only on getting profile. Organize limited campaigns with free loot boxes and many more using this cloud function and global varialbe `#!js now` to determine campaign borders.

## Developing PvE cloud functions

As mentioned before PvE mechanism depends on 3 cloud functions implemented by developer: `pveInit.js`, `pveAct.js` and `pveFinalize.js`. Consider them as set of logical steps with some share from operative memory. Function `pveInit.js` should return freshly made battle model and return it via `#!js PveInitResponse`, functions `pveAct.js` and `pveFinalize.js` has `#!js args.battleModel` global argument to work with it. 

!!! abstract "Further listed some hints and theses to help you get along with this cycle"
    - Function `pveAct.js` doesn't have: `#!js selfHumanId` global variable, access to profiles, ratings, matchmaking and locking;
    - Consider `pveAct.js` cloud function as high frequently called, so you should keep it as lightweight as possible;
    - Battle model(`#!js args.battleModel` global argument) is the core of this cycle - ==**do not ignore it**==;
    - If you have shared game library code({>>maybe transpiled from another language<<}) implement it as a set of internal cloud functions and use `#!js run()` API;
    - Use **Mersenne Twister** algorithm for deterministic random.

If PvE is enabled it'll be filled with default cloud functions. You can change them at:

 > _https://super-studio-app-dev.gbln.app/api/v0/cf.gui_

### `pveInit.js`:
```javascript
var theModel = { currentTurn: 0 };

PveInitResponse(theModel, { okay: true });
```
### `pveAct.js`:
```javascript
if(clientParams.makeTurn){
    args.battleModel.currentTurn++;
    PveActResponse(false, args.battleModel, { okay: true, currentTurn: args.battleModel.currentTurn });
} else if(clientParams.gameOver){
    PveActResponse(true, null, { okay: true, gameOver: true });
}
```
### `pveFinalize.js`:
```javascript
if(clientParams.battleJournal){
    appendSelfBattleJournalPve({ entry: clientParams.battleJournal });
}
PveFinalizeResponse();
```
It means that you can start to use PvE right away sending just 2 types of commands: 

 > `#!js gbaseApi.pve.act({ makeTurn: 1 }, callbackFn);`;

and

 > `#!js gbaseApi.pve.act({ gameOver: 1, battleJournal: { something: 'to persist' } }, callbackFn);`. 

{>>Ofcourse it's not even authoritarian but at least something<<}.

## Developing PvP cloud functions

Player versus player(_or player versus self_) gameplay ==depends on 8 cloud functions==: `pvpGeneratePayload.js`, `pvpInitGameplayModel.js`, `pvpTurnHandler.js`, `pvpConnectionHandler.js`, `pvpDisconnectionHandler.js`, `pvpCheckGameOver.js`, `pvpGameOverHandler.js` and `pvpAutoCloseHandler.js`. All of them called by Goblin Backend automatically according to particular events, you don't need to think about calling them, but you should think about life cycle of pvp gameplay and circumstances.

!!! example "Here we look at the decomposition of pvp gameplay life cycle including matchmaking"
    1. Start searching for opponent by calling `#!js matchmaking.searchPvpOpponent();` - your matchmaking state changes inside of system;
    2. Accept or decline found opponent by calline either `#!js matchmaking.acceptPvpMatch();` or `#!js matchmaking.declinePvpMatch();` - updates your matchmaking state;
    3. In case of acception, wait for opponent to accept by calling blocking function `#!js matchmaking.waitForPvpOpponentToAccept()`;
    4. Make a new connection to picked gameplay room. Gameplay room is a real host separated from the main cluster with it's own domain address and operative subsystem. To connect you use so-called *booking key* using route `/releaseBooking`. Furthermore if you're playing versus self(fictive opponent or bot) player B's payload made here by calling `pvpGeneratePayload.js` cloud function;
    5. Set your payload by sending `/setPayload` POST requests with payload json body. If direct profile exposure is disabled payload be generated by calling `pvpGeneratePayload.js` cloud function. Global arguments `#!js args.fromHid`, `#!js args.isA` and `#!js args.isBot` are available;
    6. Set ready by sending `/setReady` request. Gameplay model made here by calling `pvpInitGameplayModel.js` cloud function;
    7. Make a web socket connection with gameplay room, it causes `pvpConnectionHandler.js` cloud function call. Furthermore every connection will cause this call;
    8. Destroy web socket connection with gameplay room, it will cause `pvpDisconnectionHandler.js` cloud function call;
    9. Send turn messages over web socket connection, it will cause `pvpTurnHandler.js` cloud function call which determines how to modify gameplay model and how to respond to opponents. Furthermore all turn messages must be signed with hash and contain serial number from 1 to infinity. All turn messages are get into pair queue excluding possibility of running 2 `pvpTurnHandler.js` cloud functions at the same time;
    10. Destory web socket connection and wait enough to gameplay ttl be out and it will cause `pvpAutoCloseHandler.js` cloud function call which determines what to do and what to respond to opponents(if any chanses);
    11. A `pvpCheckGameOver.js` cloud function called automatically every time after yet another `pvpTurnHandler.js` cf call. It checks whether it's time to finish;
    12. If so, `pvpGameOverHandler.js` cloud function called which determines what to do due to the finish and what to response to opponents.

### Developing `pvpGeneratePayload.js` cloud function
A gameplay model is made of opponent's payloads - payload generation comes first. Use this cloud function to get data from profiles, watch global arguments: `#!js args.fromHid` - to see for whom payload should be made, `#!js args.isA` - to know whether target player is opponent A(is opponent B if `#!js false`), `#!js args.isBot` - to know whether target opponent is bot. If POST body provided on `/setPayload` request it will be available at `#!js args.fromObject` global argument. {>>Usually if you playing versus fictive opponent you occupy opponent A and bot is always B<<}.

!!! warning
    In terms of backend _bot_ - is not an actual AI. He'll not play with you out of the box. Used just as an abstraction doing nothing until you'll implement bot's logic with cloud functions by your self.

With PvP enabled the code base is filled with `pvpGeneratePayload.js` cloud function by default:
```javascript
if(args.isA){
    PvpResponse({ some: 'payload a', aPayload: args.fromObject });
} else if(args.isBot){
    PvpResponse({ some: 'payload b', alsoBot: true });
} else {
    PvpResponse({ some: 'payload b', aPayload: args.fromObject });
}
```
Here we set a simple payload data with `aPayload` node. It filled with POST body data. {>>It's not authoritarian but at least something<<}. You can access profiles data here on your own. ==At this cloud function you can read and write only own profile==.

### Developing `pvpInitGameplayModel.js` cloud function
Is called automatically when both payloads ready and starts gameplay itself after. The next global arguments available: `#!js args.payloadA` - opponent A's payload, `#!js args.payloadB` - opponent B's payload and `#!js args.randomSeed` - is a random seed for **Mersenne twister** algorithm. Access to databased is not available here so you should be sure that all appropriate data been taken at `pvpGeneratePayload.js`.

With PvP enabled the code base is filled with `pvpGeneratePayload.js` cloud function by default:
```javascript
PvpResponse({ playerA: args.payloadA, playerB: args.payloadB, playerA_Sequence: 0, playerB_Sequence: 0 });
```
Here we just put payloads and sequence values, we will incmenet them with turn messages. It's very casual but in real life you can use shared code library to init complicated gameplay models.

### Developing `pvpConnectionHandler.js` cloud function

Is called automatically when any opponent creates web socket connection. The next global arguments available: `#!js args.theModel` - previously generated gameplay model, `#!js args.isA` - a boolean whether player A or B connects, `#!js args.startTs` - a timestamp of gameplay begin, `#!js args.randomSeed` - previously generated  random seed, `#!js args.playerTurnA` - opponent A's turn message counter to generate turn message sign and `#!js args.playerTurnB` - opponent B's turn message counter to generate turn message sign. All this data can be useful to connected opponent because his client maybe crashed and lost all the data. CF must be ended with `#!js PvpConnectionHandler`.

With PvP enabled the code base is filled with `pvpConnectionHandler.js` cloud function by default:
```javascript
PvpConnectionHandler({
    isA: +args.isA,
    playerTurnA: args.playerTurnA, playerTurnB: args.playerTurnB,
    mdl: {
        randomSeed: args.randomSeed,
        startTs: args.startTs,
        model: args.theModel
    }
}, null);
```
Here we send all the data to connected player but nothing to opponent due to he anyway will see the service message about continuation.

### Developing `pvpDisconnectionHandler.js` cloud function

Is the opposite of connection handler and is called automatically when any opponent loses web socket connection. The next global arguments available: `#!js args.theModel` - previously generated gameplay model, `#!js args.disconnectedIsA` - a boolean whether player A or B disconnects, `#!js args.playerTurnA` - opponent A's turn message counter to generate turn message sign and `#!js args.playerTurnB` - opponent B's turn message counter to generate turn message sign. CF must be ended with `#!js PvpDisconnectionHandler`.

With PvP enabled the code base is filled with `pvpConnectionHandler.js` cloud function by default:
```javascript
PvpDisconnectionHandler({
    isA: +args.disconnectedIsA,
    playerTurnA: args.playerTurnA, playerTurnB: args.playerTurnB,
    theModel: args.theModel
});
```
Here we send useful data to still connected opponent, but he anyway will see the serivce message about going on pause.

### Developing `pvpTurnHandler.js` cloud function

Reacts on each turn message(not direct message) when it's been taken from pair queue. The next global arguments available: `#!js args.theModel` - previously generated gameplay model, `#!js args.isA` - a boolean whether player A or B sent that message, `#!js args.theMessage` - the message itself, `#!js args.playerTurnA` - opponent A's turn message counter to generate turn message sign and `#!js args.playerTurnB` - opponent B's turn message counter to generate turn message sign. CF must be ended with `#!js PvpMessageHandler` passing modified gameplay model(or negative argument if no modifications), a message that will be sended to opponent A and message that will be sended to opponent B.

With PvP enabled the code base is filled with `pvpTurnHandler.js` cloud function by default:
```javascript
if(args.theMessage.gameOver){
    let finalMessage = args.theMessage.send || { gameIsOver: true };
    PvpMessageHandler(
        { gameIsOver: true, battleJournal: args.theMessage.battleJournal || { theDefault: 'entry' } },
        finalMessage, finalMessage
    );
} else {
    let aMessageForA = args.isA ? undefined : args.theMessage.send,
        aMessageForB = args.isA ? args.theMessage.send : undefined;
    if(args.theMessage.modify){
        _.each(args.theMessage.modify, (v, k) => args.theModel[k] = v);
        PvpMessageHandler(args.theModel, aMessageForA, aMessageForB);
    } else {
        PvpMessageHandler(undefined, aMessageForA, aMessageForB);
    }
}
```
Here CF awaits for 4 nodes: `send` - what to send to opponent, `modify` - set of root nodes of gameplay model to modify, `gameOver` - says that it is time and `battleJournal` - an any-schema data to persist into battle journal. To diversify responsibilities we don't handle game over here, we only can change the gameplay model to `{ gameIsOver: true, ... }`. After that this model goes strict to `pvpCheckGameOver.js` cloud function.

### Developing `pvpCheckGameOver.js` cloud function

It has narrow resposibility set just to say whether game is over by checking the gameplay model. Global argument `#!js args.theModel` is the only available here.

With PvP enabled the code base is filled with `pvpCheckGameOver.js` cloud function by default:
```javascript
PvpResponse(args.theModel.gameIsOver ? true : null);
```
To consider game over just return plain object or any positive value

### Developing `pvpGameOverHandler.js` cloud function

Handles game over right after `pvpCheckGameOver.js`. The next global arguments available: `#!js args.opponentIsBot` - whether opponent is fictive, `#!js args.theModel` - previously generated gameplay model, `#!js args.humanIdA` - opponent A's Human ID and `#!js args.humanIdB` - opponent B's Human ID. During this CF you can get and modify profiles, ratings and add battle jounrla entries. You should finish execution by calling `#!js PvpResponse` but no argument required.

With PvP enabled the code base is filled with `pvpGameOverHandler.js` cloud function by default:
```javascript
if(args.theModel.battleJournal){
    appendBattleJournalPvp(args.theModel.battleJournal, false, !!args.opponentIsBot);
}
PvpResponse();
```
Here we write a battle journal entry only if end turn message contained `battleJournal` node. No profile modifications made. It's totally not authoritarian but at least something. Ofcourse you can do it maximum authoritarian on your own.

### Developing `pvpAutoCloseHandler.js` cloud function

Handles special cases when gameplay ends abnormally, usually when ttl is out. The next global arguments available: `#!js args.opponentIsBot` - whether opponent is fictive, `#!js args.lagA` - is time passed since last action of opponent A in milliseconds, `#!js args.lagB` - is time passed since last action of opponent B in milliseconds, `args.theModel` - previously generated gameplay model, `#!js args.humanIdA` - opponent A's Human ID and `#!js args.humanIdB` - opponent B's Human ID. Use `#!js lagA` and `#!js lagB` to determine the causer({>>whose bigger<<}), make profile changes and write battle journal entry. Must be ended with `#!js PvpAutoDefeatResponse` which gets 2 arguments - messages for both opponents.

With PvP enabled the code base is filled with `pvpAutoCloseHandler.js` cloud function by default:
```javascript
var theResult = { loserIs: args.lagA > args.lagB ? args.playerA : args.playerB };
appendBattleJournalPvp(theResult, true, !!args.opponentIsBot);
PvpAutoDefeatResponse(theResult, theResult);
```
Here we just persist battle journal entry with loser and send it to opponents additionally.