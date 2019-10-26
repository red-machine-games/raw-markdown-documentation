# Profiles

After signing up/in and before doing any work the second necessary thing you should do is to create or get the profile. During this procedure, your session is filled with useful data that allows doing further logic. A profile is separated from Account abstraction. Its goal is to keep all player's data in a schema-less manner. Basically, you should consider profile as a plain object, but it has a fixed limited amount of root nodes:

- `profileData` - Here you put all player-related data: gold, crystals, cards, army, character, progress, etc;
- `publicProfileData` - Second and the last schema-less node: here you put all data you consider public(name, level, etc.). Warning, this node can be read by any player;
- `rating` - Matchmaking related number;
- `mmr` - Matchmaking related number;
- `wlRate` - Matchmaking related number;
- `ver` - A domain version of a particular profile. More information below.

## Profile version

When you have basically schema-less profile a problem of schema looks different than relational schema. Developer should keep his schema up to date and modify it from time to time depending on the client version. Goblin Backend has a framework for profile mutation: it contains 2 things: `ver` that you should support up-to-date and `mutateProfile` cloud function(if authoritarian gameplay enabled). So you just need to match the client's version with profile `ver` and mutate profile if needed. Using `mutateProfile` cloud function the things getting easier. It will be called automatically at the right time before accessing it. Here is an example of how this function looks like:
```javascript
var laVersion = await getProfileNode('ver');
        
if(laVersion < 2){
    let mutationsCount = await getProfileNode('profileData.mutationsCount');
    setProfileNode('ver', laVersion + 1);
    setProfileNode('profileData.mutationsCount', mutationsCount + 1);
}
        
MutateProfileResponse();
```
{>>With this tool developer can code chain of mutations without bother of calling it in right time<<}

## Create new profile

The procedure for creating a profile is straightforward. You can check the profile presence with account information after signing up. During process `createNewProfile` cloud function is called if there is one. Its goal is to generate basic values for root keys `profileData` and `publicProfileData`. Besides these keys profile will have `#!js wlRate === 0` and `#!js ver === 1`. Also, special field `humanId` will be added - it is just an incrementing unique number representing `Human-readable ID`, you can't modify it, only get.
Let's see a simple example of `createNewProfile` cloud function:
```javascript
var profileData = { somePrivateData: 'space' },		// Here we prepare private data of fresh new profile
	publicProfileData = { myHumanIdVisibleForEveryone: selfHumanId };	// And public data. We can access self human ID inside of cloud function context with global variable "selfHumanId"

CreateNewProfileResponse({ profileData, publicProfileData });	// 	Mandatory response with these fields
```
But if you didn't implement `createNewProfile` cloud function there will not be fields `profileData` and `publicProfileData` in fresh profile - you can add them later.
Look at client-side procedure:
```javascript
/* Skipping GbaseApi init */

if(gbaseApi.currentAccount.prof){
	// This account already has some previously created profile. No need to call "createProfile" 
} else {
	gbaseApi.profile.create((err, response) => {
		if(err){
			console.error('Oops!');
			process.exit(-1);
		}

		// Now we have account with profile and ready to do jobs
	});
}
```

## Getting profile data

Once you've created a profile you don't need to do it further. You need to get the profile every time after signing in. Getting whole profile body in response only available with non-authoritarian gameplay(configured on backend side). When authoritarian the only thing you'll get is `#!js { disallowDirectProfileExposure: true }` - it means that you can't use any methods that directly exposures on game data, only cloud functions are available. Literally, without cloud functions, you'll be able only to manage your account. You can check the value like this:
```javascript
/* Skipping GbaseApi init */

if(gbaseApi.currentProfile){
	if(gbaseApi.currentProfile.disallowDirectProfileExposure){
		// I can run all logic only from cloud functions
	} else {
		// I can freely change my game data
		// My profile is available at gbaseApi.currentProfile
	}	
} else {
	// I need to login or create new profile
}
```

## Setting profile data

This procedure considering the full rewrite of certain root nodes. All profiles have a fixed number of root nodes shown at the beginning of the section. It's a good practice to use this procedure right after creation and once at every mutation but no more, to use networking optimally. 
Important to point that you're not able to do any profile modifications if it's direct exposure disallowed: `#!js gbaseApi.currentProfile.disallowDirectProfileExposure === true`
In the example below we will set all possible nodes:
```javascript
/* Skipping GbaseApi init */

gbaseApi.profile.setp(
	{ entirelyNewProfileData: 'spacecraft' },
	{ entirelyNewPublicData: 'rocket' },
	1, 1, 1, 2,
(err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// All profile root nodes was set to new values and new "ver" is 2
});
```

## Updating profile

More appropriate for frequent use procedure updates only particular keys of `profileData` and `publicProfileData`. As arguments you should provide each key as a full path.
```javascript
/* Skipping GbaseApi init */

// Let's imagine that we have profileData={ background: 'administration', hot: { matter: 1, parallel: 2, stereotype: 3 } }
//                  and publicProfileData={ competition: 'piece', tell: { shoulder: 1, comprehensive: 2, outline: { wrap: 3, sight: 4 } } }

gbaseApi.profile.update({ 'hot.matter': 10, 'hot.stereotype': 30 }, { 'tell.shoulder': 10, 'tell.outline.wrap': 30 }, null, null, null, null, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// As you can see each key representing full path to profile value.
	// Now we have gbaseApi.currentProfile.profileData={ background: 'administration', hot: { matter: 10, parallel: 2, stereotype: 30 } }
	//   and gbaseApi.currentProfile.publicProfileData={ competition: 'piece', tell: { shoulder: 10, comprehensive: 2, outline: { wrap: 30, sight: 4 } } }
});
```
This modification is atomic even without Goblin Backend atomic framework hence you easily can persis payments and exchanges.

## Getting public profile

The root node `publicProfileData` conceived as everything that you want to show all other players. For example your public name, level, avatar or whatever. So you should keep this data synced between nodes `profileData` and `publicProfileData`. Remember that every profile in Goblin Backend has its own unique human-readable ID and only knowing it you can query public profile data.
```javascript
/* Skipping GbaseApi init */

var theHumanIdThatWeFoundOutSomehow = 1337;

gbaseApi.profile.getPublic(theHumanIdThatWeFoundOutSomehow, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// You can see whole body of publicProfileData at response.details.originalResponse.publicProfileData
	// And you can find out target profile's version at response.details.originalResponse.ver
});
```