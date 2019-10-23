# Networking guide 🌐
If there is no ability to use prepared client plugin you need some knowledge of how to directly send requests to server. In this guide we provide information about core concepts and steps.

Client can communicate with Goblin Backend in 3 ways: over ==HTTP==, ==Web socket== and ==UDP==. And server is divided into two parts: meta-game part and pvp part.

## Communication with meta-game    
This is the part where everything begins: accounts, profiles, cloud function, etc. You can use only HTTP with part.
Backend will have address like this: 
> https://{your company abbreviation}-{your project abbreviation}-{environment: dev/qa/production}.gbln.app/api/v0/{a route name}

To successfully send requests you need 4 headers: `X-Platform-Version`, `X-Req-Seq`, `X-Request-Sign`, `X-Unicorn`:

| Header&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Description |
|--------|-------------|
| X-Platform-Version | To communicate with backend client must identify it self: send information about platform(it can be `android`, `ios`, `stdl` - standalone, `webvk` and `webok`) and its semantic version. So in final header should look like this: `X-Platform-Version: ios;0.0.2`. This identity helps backend to request client to update or to fully reject if platform is not supported. Also HMAC secret picked by this information. Don't forget to properly set platform and periodically increment semantic version of your client. |
| X-Req-Seq          | All requests are protected by HMAC sign to prevent faking. Also to protect already done requests the requests sequence is implemented. The sequence is used in sign generation and every time increments hence can't be 2 identical signs. In first requests it starts from 0. |
| X-Request-Sign     | To protect networking HMAC sign is requested by backend. It made of request data, X-Req-Seq data, a secret and X-Unicorn. HMAC secret is the most important data you should hide inside of your client build. Nobody can fake your requests until secret is found. But if it is new secret can be easily made and implemented in backend configuration - you'll just need to re hide new secret inside of new version of the build. A sign is done like this: `sign = hex(sha256(uri + body + reqSeq + unicorn + hmacSecret))` |
| X-Unicorn          | After successful login you will get an unicorn in response - it is your session key. Use it with all further requests. |


**First requests**  
Before any networking you need to make first 2 mandatory requests: get(or create new) account and get(or create new) profile. Both of them should have `X-Req-Seq` and `X-Request-Sign` but only second will have `X-Unicorn`. So lets see how these requests will look like with `Node.js`, `request` and `crypto`:
```javascript
	var request = ('request'),		// Define required modules
		crypto = require('crypto');

	// First of all let's create a new account because we don't have any credentials

	var path = `https://some-proj-dev.gbln.app`,
		requestSequence = 0,	// Starts from zero
		hmacSecret = 'A very secret bytes. Hide me fast!',
		uri;

	var gClientId, gClientSecret, unicorn;

	// Okay now let's make a request
	firstOne();

	function firstOne(){
		uri = '/api/v0/accounts.getAccount';

		// Let's make a sign
		hmacSign = crypto.createHash('sha256').update(Buffer.from(uri + requestSequence + hmacSecret), 'binary').digest('hex');

		request.post({
			url: path + uri,
			headers: {
				['X-Platform-Version']: 'ios;0.0.2',
				['X-Req-Seq']: requestSequence,
				['X-Request-Sign']: hmacSign
			}
		}, (err, response, body) => {
			if(err || response.statusCode !== 200){
				return console.error('Oh no!');
			}
			body = JSON.stringify(body);

			// Now we got new credentials as far as we signed up as anonymous - Goblin Backend made it for us
			gClientId = body.gClientId;
			gClientSecret = body.gClientSecret;

			// And we got unicorn
			unicorn = body.unicorn

			// Now we create a new profile
			secondOne();
		});
	}
	function secondOne(){
		// Increment sequence
		requestSequence++;

		uri = '/api/v0/profile.createProfile';

		// Let's make a sign
		hmacSign = crypto.createHash('sha256').update(Buffer.from(uri + requestSequence + unicorn + hmacSecret), 'binary').digest('hex');

		request({
			url: path + uri,
			headers: {
				['X-Platform-Version']: 'ios;0.0.2',
				['X-Req-Seq']: requestSequence,
				['X-Request-Sign']: hmacSign,
				['X-Unicorn']: unicorn
			}
		}, (err, response, body) => {
			if(err || response.statusCode !== 200){
				return console.error('Oh no!');
			}

			// Now let's try to set profile data. It's not necessary to do this but it will demonstrace how to sign a post request with body
			thirdOne();
		});
	}
	function thirdOne(){
		// Increment sequence
		requestSequence++;

		uri = '/api/v0/profile.setProfile';

		var body = { profileData: { goblin: 'backend' } };

		// Let's make a sign. By the way if you send body it should be in sign!
		hmacSign = crypto.createHash('sha256').update(Buffer.from(uri + JSON.stringify(body) + requestSequence + unicorn + hmacSecret), 'binary').digest('hex');

		request({
			url: path + uri,
			json: body,
			headers: {
				['X-Platform-Version']: 'ios;0.0.2',
				['X-Req-Seq']: requestSequence,
				['X-Request-Sign']: hmacSign,
				['X-Unicorn']: unicorn
			}
		}, (err, response, body) => {
			if(err || response.statusCode !== 200){
				return console.error('Oh no!');
			}

			// Done! 
			process.exit(0);
		});
	}
```
As you can see there is only 2 required things: headers and 2 initial requests. After that you can freely communicate with backend.

## Communication with PvP room

The second part of backend can be accessed on different addresses. For example if you have meta-game on `some-proj-dev.gbln.app` and you have 2 pvp rooms so you can access them in `some-proj-dev-pvp-01.gbln.app` and `some-proj-dev-pvp-02.gbln.app` respectively. Rooms can scale vertically and horizontally by farm pattern.
Usually you will get a place in pvp room after matchmaking along with information: address, port and **booking key** - everything you need to make connection with pvp room.
Before connection you need to make 3 http requests to pvp room: 

- `/api/v0/releaseBooking` - Place your self into room with given booking key;
- `/api/v0/setPayload` - Before play you need to set your battle data(you cards deck or character's weapon what ever). If you use authoritarian gameplay payload will be generated by predefined cloud function;
- `/api/v0/setReady` - Just point that you're ready to play.

All these request should contain headers `X-Platform-Version`, `X-Req-Seq`, `X-Request-Sign` and `X-Book-Key` instead of unicorn. Also request sequence will begin from zero because it belongs to particular pvp room. Further sign generated in the same way.
After releasing booking key you can make new websocket connection with pvp room on root url. Details in separate section.