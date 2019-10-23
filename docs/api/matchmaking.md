# Matchmaking

Matchmaking is a basis for players interactions. You can look it as a search by leaderboards: you search among picked leaderboard using one of two strategies(by ladder place or by exact record value). The search can be used separately from PvP or conjuncted with PvP mechanism, in first case you'll just get the opponent's **Human ID** without any further jobs. By the way, matchmaking functionality fully available both from client and from cloud functions.

## Matchming player without further acts

Is quite simple and needs only leaderboard segment and search range. It's asynchronous means that your opponent will not be affected.
```javascript
/* Skipping GbaseApi init */

gbaseApi.matchmaking.matchPlayer(
	'main_segment', 
	Gbase.GbaseApi.MATCHMAKING_STRATEGIES.BY_RATING, 
	new Gbase.GbaseRangePicker('whatever').range(Gbase.GbaseRangePicker.NEGATIVE_INFINITY, Gbase.GbaseRangePicker.POSITIVE_INFINITY),
	(err, response) => {
		if(err){
			console.error('Oops!');
			process.exit(-1);
		}
		// You'll get a response with found opponent's human ID and profile version: response.details.originalResponse={ humanId: ..., ver: ... }
	}
);
```