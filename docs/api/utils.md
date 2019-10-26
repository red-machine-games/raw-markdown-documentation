# Utilitarian methods

Some util methods that you can find useful.

## Get server time

Server is configured at **UTC+0**. The response is milliseconds-based timestamp.
```javascript
/* Skipping GbaseApi init */

gbaseApi.utils.getServerTime((err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Response at response.details.originalResponse
});
```

## Mobile in-app purchases validation

You can validate receipts from Google Play and App Store depends on the platform you configured API with. Also, it's possible to validate receipts from cloud functions.
```javascript
// Let's init API
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.ANDROID, '0.0.2');

var receiptFromGooglePlay;	// TODO Let's say you have a receipt from Google Play. 

gbaseApi.utils.purchaseValidation(receiptFromGooglePlay, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Response at response.details.originalResponse will look like { isValid: true, duplicate: false, persisted: true }
	// Where isValid is particular answer, duplicate means whether this receipt was already validated once and been persisted, persisted is self-titled
});
```

```javascript
// Let's init API
var gbaseApi = new Gbase.GbaseApi('some-proj', 'dev', 'some HMAC secret', Gbase.GbaseApi.PLATFORM.IOS, '0.0.2');

var receiptFromAppStore;	// TODO Let's say you have a receipt from App Store. 

gbaseApi.utils.purchaseValidation(receiptFromAppStore, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Response at response.details.originalResponse will look like { isValid: true, duplicate: false, persisted: true }
	// Where isValid is particular answer, duplicate means whether this receipt was already validated once and been persisted, persisted is self-titled
});
```