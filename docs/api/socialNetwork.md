# Social network things

The mechanism for acceptance of in-apps inside of VK.com and OK.ru websites is presented. Cases are similar among these two networks: you should provide them a **URL of service callback** that be accessed by their backends. E.g. if we have a project with id `superstudio-game` and `dev` environment - we should provide URL 

 > _https://superstudio-game-dev.gbln.app/api/v0/vkJobs.vkServiceCallback_
 
to VK.com and 
 
 > _https://superstudio-game-dev.gbln.app/api/v0/okJobs.okServiceCallback_
 
to OK.ru.

## Example of purchases listing

After successful purchase, data will be persisted and client lists and consumes it. Consuming is a formal act that marks the purchase as consumed, it can be done only once so you can build a domain logic around this mechanism. Listed purchases will contain a flag whether they were consumed.
```javascript
/* Skipping GbaseApi init */

gbaseApi.social.vkListPurchases(0, 20, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Can find listing at response.details.originalResponse
});
gbaseApi.social.okListPurchases(0, 20, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Can find listing at response.details.originalResponse
});
```

## Example of consuming

```javascript
/* Skipping GbaseApi init */

var vkPurchaseNum,	// Let's imagine that we have these values
	okPurchaseNum;

gbaseApi.social.vkConsumePurchase(vkPurchaseNum, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now this particular purchase will be flagged as consumed. The same will see in listing
});
gbaseApi.social.okConsumePurchase(okPurchaseNum, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Now this particular purchase will be flagged as consumed. The same will see in listing
});
```