# Cloud functions ☁️

==Is commonly used feature to easily integrate custom code into the cloud==. Goblin Backend implements them to let the client-side developers code down domain logic on backend-side in most casual way using simplified JavaScript. It specially encloses cliend-side developer from such a complicated things like infrastructural and scaling issues, querying database and operative cache, javascript's event-driven nature and more. One of the biggest needs to use cloud functions is to implement authoritarian logic and PvE bots. To prevent hacking and implement atomic changing of 2 or more entities.
Let's have a quick look at cloud function's example.

_Here we spend some gold to upgrade our card level_:
```javascript
await lock.self();	// Need to lock self before interactions. Find out more later

var myGold = await getProfileNode('profileData.resources.gold'),	// Read this data from profile
	myCards = await getProfileNode('profileData.cards');

var idOfTargetCard = clientParams.cid;	// Btw we can read client-side arguments

if(!idOfTargetCard){	// Check the argument
	return FunctionResponse({ success: false, message: 'No cid parameter' });
}

if(myGold >= 100){	// Check that player has enough gold
	myGold -= 100;
} else {
	return FunctionResponse({ success: false, message: 'Need more gold' });
}

var targetCard = myCards.find(e => e.id === idOfTargetCard);

if(!targetCard){	// Check that player has target card and increment level
	return FunctionResponse({ success: false, message: 'No such card' });
} else {
	targetCard.level++;
}

setProfileNode('profileData.resources.gold', myGold);	// Persist it
setProfileNode('profileData.cards', myCards);

FunctionResponse({ success: true });	// Report success
```

That was the basic example of authoritarian domain logic implemented with cloud function. We'll go further to learn more cool thing you can reach with it.

## ACID engine

Cloud functions can make some changes to database and operational data so to not to lose updates on half way it should be guaranteed by transactional engine. Inside of cloud function you can change profile, ratings, validate store receipt, add battle journal entries for your self or for many other players.

!!! example "How transaction life cycle looks like"
	1. Before making any changes you must lock your self or some people to prevent other invading your updates using methods of object `#!js lock`: i.g. `#!js await lock.self();` or `#!js await lock.selfAnd([someHumanId]);`, `#!js await lock.some([someHumanId, anotherHumanId]);`. Also you can use `#!js relock` instead of `#!js lock` but better to not use it without strict need;
	2. Lock is deadlock-less due to you can't get a lock already having lock also locker is single threaded and race-less so if you wait for somebody, nobody waits for you;
	3. After getting lock you can operate with data: read and update. ==Warning! Updating is not done at the moment, all updates are done only after whole cloud function is done==. So you'll read some value from profile, push update and read again, it will remain the same. Updated value will be available only after cloud function done;
	4. All set and update functions are filling the transaction entity with data. Transaction entity is a document that represents exhaustive set of data and as soon as it'll be persisted transaction guaranteed to succeed. Even in case of error all the data will cyclically roll on till success. No entities with undone transactions are available. The key is idempotency of updates;
	5. The lock freed and request done after successful updates.

## CRUD them

You can "CRUD" cloud functions with simplified webpage. Let's imagine we have a project with ID `super-studio-app` and `dev` environment so webpage available at:

 > _https://super-studio-app-dev.gbln.app/api/v0/cf.gui_

!!! example "The next list of theses is true for _CRUD_-ing cloud functions"
	1. Actually you can't upload or update particular separately taken functions to not to interrupt consistency. Uploading happens only for whole code base totally replacing previous;
	2. But you can delete separately taken scripts;
	3. Routes are protected with basic auth. The credentials be found during deploy process.

You should pay big attention to updating process. It's frequent when code base represents whole consistent entity and inconsistency appears while updating due to horizontally scaled system design. So just imagine that you have a sequence of meta actions so at the moment of update client has a chance to call acts of different versions. Here you can find maintenance mechnaism helpful. See further to find out more about maintenance.

## Maintenance

Is useful not only for cloud functions but for any request from client. Webpage can be found at 

 > _https://super-studio-app-dev.gbln.app/api/v0/utils.maintenanceGui_
 
It works very simple: set the wildcard-based filter for URIs that should be blocked and they will respond with http code **503** and error `#!js { index: 963, message: 'Under maintenance now!' }`.

## Predefined cloud functions and user-defined ones

All cloud functions should have unique names satisfying regexp `#!js /^[$A-Z_][0-9A-Z_$]*$/i`({>>just camel-case without extra chars<<}). Moreover Goblin Backend has list of predefined cloud functions that corresponds to particular features. So for example there are 3 cloud functions to run PvE and 8 to support PvP. All of them are called in particular order and are not available for external calls. 

!!! abstract "Further we see the list of predefined cloud functions with descriptions"
	- `initContext.js` - is called automatically when cluster starts to pre init some global variables through global property `#!js glob` available from all cloud functions. It's not necessary and moreover recommended to call custom cloud function directly from other cloud functions. ==Disabled by default==;
	- `createNewProfile.js` - is called automatically before creation new profile. It should return an object as primary filling for new profile;
	- `mutateProfile.js` - a special and very useful cloud function called every time before accessing any player's profile. Allows to mutate profile before use {>>find out more about mutation at Profile section<<};
	- `onGetProfile.js` - is called automatically before getting profile. Useful to do some system modifications on it. Not necessary to implement. ==Called after mutation==. Also session cache is available via `#!js session` global variable - it's a great place to heat up your cache;
	- `pveInit.js` - is called on `#!js gbaseAPI.pve.begin()` and used to init operative gameplay model;
	- `pveAct.js` - is called on `#!js gbaseAPI.pve.act()`. Used to make updates on gameplay model and check whether game is over;
	- `pveFinalize.js` - is called automatically after `pveAct.js` if it said that game is over. Used to persist some profiles updates and battle journal entries;
	- `pvpGeneratePayload.js` - is called before PvP starts to generate opponent's payload. Need to check argument `#!js args.isA` - to check whether it's about player A or B. And argument `#!js args.isBot` in case you're playing versus self. It's up to you to generate whatever payload you want;
	- `pvpInitGameplayModel.js` - is called before PvP starts and after payloads generation to generate operative gameplay model it self. Payloads are available at `#!js args.payloadA` and `#!js args.payloadB`;
	- `pvpConnectionHandler.js` - is called automatically against just connected opponent to form a message for him;
	- `pvpDisconnectionHandler.js` - is called automatically against just disconnected opponent to form a message for his opponent;
	- `pvpTurnHandler.js` - handles every single _turn message_. Results with 3 arguments: modified gameplay model, message for opponent A and message for opponent B;
	- `pvpCheckGameOver.js` - called right after `pvpTurnHandler.js` to check whether game is over. Checks incoming argument `#!js args.theModel` and outputs the only one argument - game over message: if presence and negative value(mostly `#!js null`) if game is not over;
	- `pvpGameOverHandler.js` - called right after `pvpCheckGameOver.js` in case of positive response. Modify profiles and add battle journal entries here;
	- `pvpAutoCloseHandler.js` - also a finalizing cloud function but called in case of automatical game over. E.g. timeout of one or both opponents. Inputs arguments `#!js args.lagA` and `#!js args.lagB` representing opponent's time passed since last ping or action. Okay to determine causer of game over in case you want to punish him. Outputs 2 arguments - a messages for opponents that would be sent if possible;
	- `onMatchmaking.js` - in case of authoritarian matchmaking(configured on backend) all arguments goes into this cloud function firstly to give you control over matchmaking. Outputs these three arguments.

!!! hint "Goblin Cloud instance already equipped with `onMatchmaking.js` cloud function"
	All the arguments provided through `#!js clientParams` and practically it searches forward or backward depending on request body.

_Here it's body_:
```javascript
if(clientParams.mmDetails && clientParams.mmDetails.whereToSearch){
	let myRating = await getSelfRating(clientParams.segment);
	if(typeof myRating !== 'number'){
		return OnMatchmakingResponse('Don\'t have rating in this segment').asError();
	}
	if(clientParams.mmDetails.whereToSearch === 'forward'){
		OnMatchmakingResponse(
			clientParams.segment, clientParams.strategy,
			{ rgs: [{ from: myRating, to: '+inf' }], nran: 10 }
		);
	} else {
		OnMatchmakingResponse(
			clientParams.segment, clientParams.strategy,
			{ rgs: [{ from: myRating, to: '-inf' }], nran: 10 }
		);
	}
} else {
	OnMatchmakingResponse('Don\'t understand you!').asError();
}
```

Also there are some reserved names for cloud functions that can't be taken and will be used in further updates:

 - `onCyclicRound.js`;
 - `onCustomAuth.js`;
 - `pvpOnEnterFrame.js`;
 - `pvpOnExitFrame.js`;