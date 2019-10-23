# Accounts Linkage

You can link account previously created with `gClientId` and `gClientSecret` with ==Facebook==, ==VK.com== or ==OK.ru== social profile. You just need to provide token from social network's SDK. This procedure will find already created profile by any account that used appropriate social network credentials. Or will just make new profile. Originaly created profile will be still alive but you will be able to access it only after unlinking your account from any social profiles. After this procedure you need to sign in again. It's possible to link by social token without creating new profile - your account become bi-login(gClientId/gClientSecret or social token)

## Link Facebook profile

You need to get a token from ==Facebook's SDK==.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.ANDROID, '0.0.1');

var fbToken; // You will receive this value from Facebook SDK - see Facebook documentation for more information

gbaseApi.account.linkFbProfile(fbToken, false, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// You have same account but pointing into new profile.
	// But relogin needed
});
```

## Link VK.com profile

You need to get a token from ==VK.com SDK==.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.ANDROID, '0.0.1');

var vkToken; // You will receive this value from VK.com SDK - see VK.com documentation for more information

gbaseApi.account.linkVkProfile(vkToken, false, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// You have same account but pointing into new profile.
	// But relogin needed
});
```

## Link OK.ru profile

You need to get a token from ==OK.ru SDK==.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.ANDROID, '0.0.1');

var okToken; // You will receive this value from OK.ru SDK - see OK.ru documentation for more information

gbaseApi.account.linkOkProfile(okToken, false, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// You have same account but pointing into new profile.
	// But relogin needed
});
```

## Unlink account from any social profile

A simple procedure to unlink from any. After this procedure account will link with originaly created profile and you need to repeat signing in but with gClient- credentials.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.ANDROID, '0.0.1');

gbaseApi.account.unlinkSocialProfile((err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Your account now pointing to originaly created profile
	// Relogin needed
});
```

## Check whether VK.com linked profile exists

Checks if there any profile exists linked with particular VK.com profile.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.STDL, '0.0.3');

var vkToken; // You will receive this value from VK.com SDK - see VK.com documentation for more information

gbaseApi.account.hasVkProfile(vkToken, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Answer in response
});
```

## Check whether OK.ru linked profile exists

Checks if there any profile exists linked with particular OK.ru profile.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.STDL, '0.0.3');

var okToken; // You will receive this value from OK.ru SDK - see OK.ru documentation for more information

gbaseApi.account.hasOkProfile(okToken, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Answer in response
});
```

## Check whether Facebook linked profile exists

Checks if there any profile exists linked with particular OK.ru profile.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.STDL, '0.0.3');

var fbToken; // You will receive this value from Facebook SDK - see Facebook documentation for more information

gbaseApi.account.hasFbProfile(fbToken, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Answer in response
});
```

## Link VK.com profile without creating new one

Works only if you don't have any VK.com profile previously created. It's natural to call `hasVkProfile` firts for check and link after. Makes your account bi-login i.e. login with id/secret as well as with VK.com token. Also no new profile will be created hence no progress "lost". Should warn that `unlinkSocialProfile` will not unlink bi-login account any more.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.ANDROID, '0.0.1');

var vkToken; // You will receive this value from VK.com SDK - see VK.com documentation for more information

gbaseApi.account.linkVkProfile(vkToken, true, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// You have same account and profile.
	// But relogin needed
});
```