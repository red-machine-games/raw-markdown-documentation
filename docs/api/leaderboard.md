# Leaderboard and records 🏆

Leaderboards are great unit of social competition. You can post records into white list of segments and retrieve leaders. All values must be ==**from 0 to 2147483647**==. Along with different segments you can retrieve leaders among your social friends - you need to push friends list manually. Also public route for retrieving leaders is available and configurable on the backend side.

## Posting record

For post you should provide segment name and positive int value itself. All segments should be whitelisted on backend side to avoid pollution.
```javascript
/* Skipping GbaseApi init */

gbaseApi.leaderboards.postRecord(1337, 'main_segment', (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Leaderboard of segment "main_segment" is now filled with one record
});
```

## Getting self record

Exact the same you'll need a whitelisted segment name. Absence of records in some requested segment is accompanied by code _404_ response.
```javascript
/* Skipping GbaseApi init */

gbaseApi.leaderboards.getSelfRecord('main_segment', (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}
	if(response.details.originalStatus === 404){
		// It's okay but you don't have a record in this segment
	} else {
		// You got the record available at "response.details.originalResponse.rec"
	}
});
```

## Get overall leaderboard

You can list leaders in `descending` order up to 20 records per time using pagination mechanism. Also leaders can retrieved publically with special route if configured. Pagination is controlled by arguments `skip` - from 0 to +infinity and `limit` - from 1 to 20. Response has 2 keys: `records` - containing array of pairs `score`+`hid`(human-readable ID) and `len` - total number of records in leaderboard of particular segment.
Let's imagine that we have a project with id `superstudio-game` and `dev` environment so we can retrieve records from segment `def` publically using the url: 

 > _https://superstudio-game-dev.gbln.app/api/v0/pub.getLeadersOverall?skip={from 0 to +infinity}&limit={from 1 to 20}&segment=def_

```javascript
/* Skipping GbaseApi init */

gbaseApi.leaderboards.getLeaderboardOverall('main_segment', 0, 20, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Here we'll get our lonely record response.details.originalResponse={ records:[{ score: 1337, hid: our hid }], len: 1 }
});
```

## Remove self record

Along with adding you can remove self record from particular segment.
```javascript
/* Skipping GbaseApi init */

gbaseApi.leaderboards.removeSelfRecord('main_segment', (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now our leaderboard is empty
});
```

## Get someone's rating

Ratings are available for everybody so you can easily see rating data of particular user having only his `Human ID`.
```javascript
/* Skipping GbaseApi init */

var someonesHumanId;	// We got this variable from somewhere

gbaseApi.leaderboards.getSomeonesRating(someonesHumanId, 'main_segment', (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now we can access target rating at response.details.originalResponse.rec
});
```

## Retrieving records among social friends

Before successful retrievning you should push appropriate network's friends list. It will cache them for a while then just repeat procedure. To get social friends you can use your desirable SDK. After pushing you'll be able to retrieve records among your friends. This feature covers 3 social networks currently: ==Facebook==, ==VK.com== and ==OK.ru==.
Friends cache live for a week(can be configured on backend) so try to refresh it at least once a week. If no friends cache returning listing will be empty. 

## Pushing Facebook friends

To push friends you should fit them into array also make a `crc32 hash` of them. {>>It's done by js asset<<}.
```javascript
/* Skipping GbaseApi init */

var myFbFriends = [123, 456, 789];	// Let's imagine that it is your friends list

gbaseApi.leaderboards.refreshFbFriendsCache(myFbFriends, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Refreshed!
});
```

## Pushing VK.com friends

To push friends you should fit them into array also make a `crc32 hash` of them. {>>It's done by js asset<<}.
```javascript
/* Skipping GbaseApi init */

var myFbFriends = [123, 456, 789];	// Let's imagine that it is your friends list

gbaseApi.leaderboards.refreshVkFriendsCache(myFbFriends, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Refreshed!
});
```

## Pushing OK.ru friends

To push friends you should fit them into array also make a `crc32 hash` of them. {>>It's done by js asset<<}.
```javascript
/* Skipping GbaseApi init */

var myFbFriends = [123, 456, 789];	// Let's imagine that it is your friends list

gbaseApi.leaderboards.refreshOkFriendsCache(myFbFriends, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Refreshed!
});
```

## Getting leaders among social friends

First of all you need to be linked with particular social profile because method is universal.
```javascript
/* Skipping GbaseApi init */

gbaseApi.leaderboards.getLeadersWithinFriends('main_segment', 0, 20, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Here we'll get our leaderboard response.details.originalResponse={ records:[{ some records }], len: some length }
});
```