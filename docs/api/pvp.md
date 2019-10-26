# Player versus player ⚔️

It is the ultimate mechanism of the player's interaction where they meet each other face to face and compete in real-time. Goblin Backend offers ==1 versus 1== matchmaking and real-time PvP as a full path from searching each other till the game over and simplified with client-side assets.
PvP infrastructure itself organized as ==farm of rooms== where the room is a real host that can fit some limited amount of pairs. Rooms can be added or removed what gives smooth scalability and during matchmaking, your pair will get the freest room at the moment. 

!!! info "Next list of theses will help you to get along with Goblin's PvP functionality"
    - Matchmaking happening in real-time means that you and your opponent must search at the same time;
    - There are literally 3 modes of play: player versus another player, player versus bot opponent and player versus self;
    - When picking versus self, you playing alone in pair with the same behavior - it's useful if you want to add _fake multiplayer_ due to immature players base;
    - PvP room supports 3 protocols: **HTTP**, **WebSocket** and **UDP**;
    - To make PvP enabled you must implement next cloud functions: `pvpGeneratePayload.js`, `pvpInitGameplayModel.js`, `pvpTurnHandler.js`, `pvpConnectionHandler.js`, `pvpDisconnectionHandler.js`, `pvpCheckGameOver.js`, `pvpGameOverHandler.js` and `pvpAutoCloseHandler.js`;
    - Successful matchmaking give you PvP room's **address** and **booking key**;
    - Only 7 routes exposures _http_: `/v0/releaseBooking`, `/v0/setPayload`, `/v0/setReady`, `/v0/surrender` plus 3 utilitarian;
    - The whole gameplay is twisted around three things: **Gameplay model**, **Start timestamp** and **Random seed**. They give you a basis to build authoritarian deterministic gameplay;
    - Opponent's payloads are used to build model, model being built on client-sides of player A and player B and on backend-side all separately. Before the game starts and in some moments you'll get opponent's payload to be able to build or rebuild a model;
    - _Mersenne twister_ algorithm is used to pick random values - use it along with **Random seed** received from PvP room;
    - Each opponent should make 3 HTTP requests to start game: `/v0/releaseBooking` - to get into, `/v0/setPayload` - to transfer payload data and `/v0/setReady`;
    - Route `/v0/surrender` used only if you unable to continue the game({>>you should think about hanging sanctions on such guy<<});
    - You'll communicate with the opponent through 3 methods: **web socket turn message**, **web socket direct message** and **UDP direct message**;
    - Web socket turn messages are used to spread about key actions. It modifies the model and calls cloud function `pvpTurnHandler.js`. All turn messages are processed though pair queue one by one means that pair gameplay model can't be modified in parallel. The computing path is long and it's desirable practice to send these messages ==not so frequent - up to 1 message per second==;
    - Web socket direct messages are used for high-frequency communications and don't call any cloud function and sent to the opponent directly that makes it lighter than turn messages. ==It's desirable practice to send these messages up to 15 times per second==;
    - Direct UDP messages alike direct messages sent directly to the opponent and don't call any cloud function but due to protocol lightness are most lightweight messages to use during real-time multiplayer. _Not supported in browsers_;
    - AFK players are disconnected and in case of opponent disconnection, the other stands on pause. Unpaused games without one of the opponent's activity go to pause and paused games due to long enough pause time finishes and calls `pvpAutoCloseHandler.js`;
    - The normal gameplay session ends with calling a `pvpGameOverHandler.js` cloud function in which you can modify profiles, records and add battle journal entries;
    - If your client crashes or refreshes or somehow lost the current game you can grab data about the current PvP match and reconnect to it.

## Winding up PvP

All steps are covered by an asset. You can start a game with a real-life opponent, bot opponent or your self({>>both are literally PvE but using same PvP mechanics<<}).
Start playing with a real opponent. Both should run the same code at the same time and approach each other by rating:
```javascript
gbaseApi.pvp.withOpponent(
	'main_segment', Gbase.GbaseApi.MATCHMAKING_STRATEGIES.BY_RATING,
    new Gbase.GbaseRangePicker('any').range(Gbase.GbaseRangePicker.NEGATIVE_INFINITY, Gbase.GbaseRangePicker.POSITIVE_INFINITY),
    60, (err, response) => {
		if(err){
			console.error('Oops!');
			process.exit(-1);
		}

		// If everything okay pvp lies at response.details.pvp
		// And response.details.originalResponse.stat equals to "MM: gameroom allocated"
	}
);
```
Gameplay versus self is more simple because it's one gate play. The interesting thing about playing versus self is that you can pass some hypothetical opponent **Human ID**. It'll not affect anybody just passing to `pvpGeneratePayload.js` cloud function. Gives you the ability to play against somebody without his actual participation. It's called _asynchronous multiplayer_ on game design slang.
```javascript

var hypotheticalOpponentHumanId;	// Let's imagine we want to play against some guy without his actual participation

gbaseApi.pvp.beginVersusSelf(beginVersusSelf, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// If everything okay pvp lies at response.details.pvp
	// And response.details.originalResponse.stat equals to "MM: gameroom allocated"
});
```

## PvP instance events

Pvp instance is an event emitter so you should listen to them for proper implementation. Further, we see the list of events with descriptions:

- `progress` - A connection progress event fires only on the connection phase before actual gameplay. Dispatches string messages as one argument. You can use it to render progress bar, but it's the natural case if won't be any `progress` events at all;
- `begin` - important and mandatory event means that the game started. From this point you'll know the variables: *opponent's payload*, *whether you are player A or B*, *random seed* and *start timestamp*
- `error`;
- `model` - system message with model-related data. It commonly fires after connection and message constructed with `pvpConnectionHandler.js` cloud function;
- `sync` - a rare message means suspicion that you desynchronized with backend. It's up to you to implement the sync mechanism;
- `turn-message` - a turn message from the opponent;
- `direct-message` - a direct message from the opponent;
- `paused` - gameplay went to pause due to opponent's disconnection;
- `unpaused` - gameplay went back from pause due to opponent's connection;
- `finish` - means that PvP is done. Has an argument with the final message.

## Sending messages

Is a piece of cake:
```javascript
pvpInstance.sendTurn({ m: 'Hello' });	// Send turn
pvpInstance.sendDirect({ someCoordinates: { x: 22, y: 33 } });	// Send direct
```

[^]: Sending UDP messages is not covered in doc yet.