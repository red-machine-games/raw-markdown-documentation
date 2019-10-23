# Accounts

Account and profiles are two different things linked with each other. Accounts represeting only data for login. You should see accounts as login methods, for example player has 1 profile linked with social profile(maybe Facebook) and 2 devices so he will have 2 account(for each device) and 1 active profile(actually each account will have it's own base profile but they both will be inactive until unlinking from social profile). Also it's possible to make account bi-login: a simple linking with social profile but keeping the one account-profile pair. You will be able to login with gClientId/gClientSecret and token from social network into the same account. But this procedure irreversible.  
Accessing account is the first initial action that you must do before any work with Goblin Backend - it will give you a fresh session. Next we will look at all variants of signing up and signing in.

## Anonymous sign up

If you're developing standalone app or mobile app the case for you to anonymously sign up - Goblin Backend will create account with credentials(gClientId and gClientSecret) for you - you'll get it in response. You should consider saving this credentials somewhere: persist on device or use side services like Google Play Services.  
Also it's the most straightforward method of creating account:
```javascript
// Let's init API
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.STDL, '0.0.2');

// And sign up
gbaseApi.account.signupGbaseAnon((err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Let's persist it
	localStorage.setItem('gClientId', response.details.originalResponse.gClientId);
	localStorage.setItem('gClientSecret', response.details.originalResponse.gClientSecret);

	// Also we can get credentials from API instance:
	localStorage.setItem('gClientId', gbaseApi.currentAccount.gClientId);
	localStorage.setItem('gClientSecret', gbaseApi.currentAccount.gClientSecret);

	// Now we are in!
});

```

After successful enter you can see account fields at `gbaseApi.currentAccount`:

 - `gClientId` _string_ - If presence
 - `gClientSecret` _string_ - If presence
 - `unicorn` _string_ - Current session token
 - `fb` _string_ - Facebook ID
 - `vk` _string_ - VK.com social network ID
 - `ok` _string_ - OK.ru social network ID
 - `prof` _boolean_ - Flag whether this account already has profile

## Sign up with custom credentials

Along with anonymous you can provide your own credentials but they should follow some rules: gClientId should correspond to RegExp `#!js /[A-Za-z0-9._@]{4,32}$/` and gClientSecret should correspond to RegExp `#!js /[A-Za-z0-9._@]{6,64}$/`. You can use email or nickname or whatever you want as gClientId
```javascript
// Let's init API
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.STDL, '0.0.2');

// And sign up
gbaseApi.account.signupGbaseCustomCredentials('login', 'password', (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}
	
	// Now we are in!
});

```

## Sign in

A simple sign in procedure using previously generated `gClientId` and `gClientSecret`.
```javascript
// Let's init API
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.STDL, '0.0.2');

// Let's get credentials
var gClientId = localStorage.getItem('gClientId'),
	gClientSecret = localStorage.getItem('gClientSecret');

// And sign in
gbaseApi.account.signinGbase(gClientId, gClientSecret, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now we are in!
})

```

## Authenticate with web VK

If you are building app for WEBVK that will be deployed into VK.com's iframe you can authenticate using credentials from it. The platform will give you an ID and generated secret, you should use to prove your identity. Also backend must be configured with server-side data: ==VK.com's app ID and app secret==. A profile made after authentication can be linked onto standalone accounts using token of ==VK.com's SDK==.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.WEBVK, '0.0.1');

var vkId, vkSecret; // You will receive this values from iframe container - see VK.com documentation for more information

gbaseApi.account.authWebVk(vkId, vkSecret, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now we are in!
	// This account will not have gClientId and gClientSecret. The only way to get in is to use VK.com credentials
});
```

## Authenticate with web OK

If you are building app for WEBOK that will be deployed into OK.ru's iframe you can authenticate using credentials from it. The platform will give you an ID and generated secret, you should use to prove your identity. Also backend must be configured with server-side data: ==OK.ru's app ID and app secret==. A profile made after authentication can be linked onto standalone accounts using token of ==OK.ru's SDK==.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.WEBOK, '0.0.1');

var okId, okSecret, okSessionKey; // You will receive this values from iframe container - see OK.ru documentation for more information

gbaseApi.account.authWebOk(okId, okSecret, okSessionKey, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now we are in!
	// This account will not have gClientId and gClientSecret. The only way to get in is to use OK.ru credentials
});
```

## Authenticate with VK SDK

If you are building standalone or mobile device app you still have method to authenticate with social profile using it's SDK. Only thing you need is a token from ==VK.com SDK==.
You can access already created profile if you did this previously using Web VK authentication.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.IOS, '0.0.1');

var vkToken; // You will receive this value from VK.com SDK - see VK.com documentation for more information

gbaseApi.account.authVkSdk(vkToken, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now we are in!
	// This account will not have gClientId and gClientSecret. The only way to get in is to use VK.com SDK
});
```

## Authenticate with OK SDK

If you are building standalone or mobile device app you still have method you authenticate with social profile using it's SDK. Only thing you need is a token from ==OK.ru SDK==.
You can access already created profile if you did this previously using Web OK authentication.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.IOS, '0.0.1');

var okToken; // You will receive this value from OK.ru SDK - see OK.ru documentation for more information

gbaseApi.account.authVkSdk(okToken, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now we are in!
	// This account will not have gClientId and gClientSecret. The only way to get in is to use OK.ru SDK
});
```

## Authenticate with Facebook token

Unlike VK and OK Facebook logins only with token. You'll get it from SDK or facebook.com iframe.
```javascript
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.STDL, '0.0.1');

var fbToken; // You will receive this value from Facebook SDK - see Facebook documentation for more information

gbaseApi.account.authFb(fbToken, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now we are in!
	// This account will not have gClientId and gClientSecret. The only way to get in is to use Facebook token
});
```

## The session

All networking is done only with user session that's tend to rot. After some time of inactivity(configured on backend side) session disappears and you'll need to re login. To prevent it you need to ping server from time to time. If using client asset it will do it automatically but you need to call method of "some user input":
```javascript
/* Skipping GbaseApi init */

gbaseApi.userInputBeenDone();	// You can call it every time some mouse/joystick or keyboard activity being done
```
