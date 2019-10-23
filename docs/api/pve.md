# Player versus environment 

A simplified PvE framework is presented. It presents a set of actions with begin and finish as 3 cloud functions: `pveInit.js`, `pveAct.js` and `pveFinalize.js`, all they are programmed. Any PvE method is based on http protocol so you should call `pveInit` no more than once per second or even less often. All this cycle looks like a set of cloud functions calls but `pveAct.js` does't allow to use database and all of them offer a big schemaless subsession where you can store all the PvE battle data.
To make this mechanism available you should upload all 3 cloud functions.

## PvE initialization

A cloud function has the same features but at the end `#!js PveInitResponse` must be called. First argument is an object - a model or some data shared across all pve cycle. The second arguments is data that will go to caller.
```javascript
var turnsToFinish = 15,
	theModel = { turnsToFinish, currentTurn: 0 };

PveInitResponse(theModel, { turnsToFinish });
```

## PvE act

This cloud function has no access to database because conceived as frequently called one. Instead you have to operate with model. At the end `#!js PveActResponse` must be called. First argument is boolean whether game is over, if it is the `pveFinalize` cloud function will run automatically and cycle stops. Second is modified model object, the third is data that will go to caller.
```javascript
if(++args.battleModel.currentTurn === args.battleModel.turnsToFinish){
    return PveActResponse(true, null, { over: true });
} else {
    return PveActResponse(false, args.battleModel, { okay: true, turn: args.battleModel.currentTurn });
}
```

## PvE finalize

This cloud function called automatically when yet another `#!js PveActResponse` called with first argument equal `#!js true`. Here you can put some data into player's profiles and make an entry into battle journal. At the end `#!js PveFinalizeResponse` must be called but inputs no arguments. Persisting profile update and battle journal entry simultaneously is guaranteed to be atomic by Goblin atomic acts framework.
```javascript
appendSelfBattleJournalPve({ iAmThe: 'law' });

PveFinalizeResponse();
```

## Passing the PvE gameplay

From the clien's side only two methods related to gameplay itself are available, plus method for listing battle journal. So according to previously added cloud code we can see that we should make 15 pve acts to finish it.

**Winding PvE up**:
```javascript
gbaseApi.pve.begin({ some: { begin: 'params' } }, (err, response) => {	// This data will go into pveInit cloud function
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now we at pve. You can't start another PvE before finishing current
});
```

**Making these acts**:
```javascript
// Let's use some iterator valiarlbe
var i = 0;

function doTheAct(){
	gbaseApi.pve.act({ some: 'param' }, (err, response) => {	// This data will go into pveAct cloud function
		if(err){
			console.error('Oops!');
			process.exit(-1);
		}

		// Response in response.details.originalResponse be equal to { okay: true, turn: i + 1 } and { over: true } for fifteenth act - the last

		if(++i === 15){
			// Go further
		} else {
			doTheAct();
		}
	});
}

doTheAct();
```

Fifteenth act will finalize PvE battle by calling `pveFinalize.js` cloud function. According to it we now can check out battle journal and see entry:
```javascript
gbaseApi.pve.listBattles(0, 20, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Journal listing at response.details.originalResponse.l
});
```