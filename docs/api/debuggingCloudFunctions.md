# Debugging cloud functions 

==Due to it is regular code base you should test and debug it==. But the circumstances are not so comfortable as for local code base - you can't debug cloud function with breakpoints but still a few tools and tricks are available to make things easier. Further we'll list tools and some theses.

## Logs facade

Use logs to simply log from cloud functions. They don't produce anything by themselfs, here's available log methods:

```javascript
log.debug('A debug message', { trust: 'excuse' });
log.info('An info message', { peanut: 'reluctance' });
log.warn('A warn message', { relieve: 'restaurant' });
log.error('An error message', { moment: 'shave' });

FunctionResponse({ okay: true });
```

All log methods gets 2 arguments: a message and payload object(_it should be an object or null_). Here is list of actions performed:

 - 1 - `#!js [INFO] Run CF` message spawned if there any `#!js log` call. 2 fields to notice - message and meta:
```javascript
{
    name: 'A name of cloud function without extension',
    rts: 1563801613563, // A timestamp of function run
    ets: 1563801613573, // A timestamo of function end
    dur: 10,    // Duration in milliseconds
    pid: 1,     // A PM2 process ID which ran this CF
    host: 'some-host-dev-harm-01',  // A host name where CF was run
    cfhash: '23de6c5e'  // Very important node representing hash of call. All logs from current call will have same node, the only way to list them
}
```

 - 2 - All log messages spawned with meta:
```javascript
{
    ms: 10, // On what millisecond of run did the log appear
    cfhash: '23de6c5e'  // The same value as on "Run CF" message. Use it to get further data about CF call
}
```

 - 3 - Logs be stored in database for about a week(_can be configured_). ==Don't use logs without strict need==. ==Logs are written without guarantee hence some of them can be lost without any notice - keep it in mind==;
 - 4 - Use _GUI_ to see logs, use _MongoDB query language_ to query them(find out more about query here: [MongoDB find documentation](https://docs.mongodb.com/manual/tutorial/query-documents/));
 - 5 - Let's imagine we have a project with ID `super-studio-app` and `dev` environment so _GUI_ is available at:

 > _https://super-studio-app-dev.gbln.app/api/v0/cf.logs_

## Traces

Is a small trick to retrieve additional logs along with actual response. Call `#!js trace('Anything to trace')` to append array of traces, add whatever data it'll appear under `#!js _traces` node in response body:

```javascript
trace('Hello');
trace('World');

FunctionResponse({ okay: true });   // Response body will be like { okay: true, _traces: ['Hello', 'World'] }
```

## Keep your cloud functions tested

All code should be provided with automated tests, testing their separated behavior and whole system behavior with real-life scenarios - usually it cuts about **90%** of bugs before getting to QA team. You should never rely on QA team only due to common case of not providing QA automation. Far not so many teams can automate QA and see it profitable hence you'll face the `Fixed 1 bug, produced 10 new` permanent issue. Don't be tricked by thought that _automated testing is pointless - cases are too many to cover them all_, just implement it and you'll be surprised how many primitive mistakes you can avoid. `Goblin Backend` is covered with about 5700+ test cases (_atm_) and it helps to avoid hundreds of ridiculous mistakes every day.

As far as Goblin Backend environment is not available in public yet you should test cloud functions locally mocking the API if needed. Use `Node.js` of _LTS_ version, packages `mocha` and `chai` to build and run test cases. Use `jsonpath` package to imitate databases behavior({>>Test case generator with all the mocks is under development right now<<}).

During development you can face a few issues due to javascript eco system({>>C# cloud function are on the way<<}). It's powerful language to skyrock the development, write literally less code and implement features with more ease and freedom. But [with great power comes great responsibility](https://en.wikipedia.org/wiki/With_great_power_comes_great_responsibility) so you should always watch out for underwater bugs and test your code properly. Here some examples:

 - Let's imagine you want to convert some `crystals` into `gold` like _10 gold per crystal_. So you write:

```javascript
await lock.self();

var myCurrentGold = await getProfileNode('profileData.myGold'),
    myCurrentCrystals = await getProfileNode('profileData.myCrystals');

setProfileNode('profileData.myGold', myCurrentGold + myCurrentCrystals * 10);
setProfileNode('profileData.myCrystals', 0);

FunctionResponse({ yay: true });
```

It's a basic meta exchange but let's imagine that developer made a mistake: crystals are stored at node `profileData.crystals` but not at `profileData.myCrystals` and variable `#!js myCurrentCrystals` equals to `#!js undefined` - what you'll get? At the end `#!js NaN` will be persisted at `profileData.myGold` node means that **player lost all gold**. The more players face this new function the more will lose gold data and unpredictable how far it'll go before QA team notice it. To prevent this scenario you should keep your functions tested and if there is no schema for profile it doesn't mean that you can't determine schema by your own. Implement any schema checks that will be comfortable for your particular pipeline;

 - A part of API methods are `#!js async` - it means that you must use keyword `#!js await` calling it. It's a strict rule and it's unpredictable what problems you'll face otherwise;
 - _Node.js_ uses `#!js Number` to represent _integral_ or _floating point_ but keep in mind that it processing it differently from other virtual machines(from any client-side environment). By using dividing operations or operations with fractional values the results may far differs from client-side results. Try to use _integral_ values as frequently as possible. ==Even one little mistake can negate everything if you're mirroring authoritarian logic==;
 - Always check values for _nullness_ like `#!js myValue === null` and for _undefinedness_ like `#!js myValue === undefined` or for both like `#!js myValue == null`.

## App Store and Google Play receipts validation

It's very common to make mistakes with receipts schemas. 

!!! example "Find out more at store's documentation and here are some hints to follow"
     - Try to log all failed receipts for further research;
     - Keep the separate store dev account to test purchases;
     - Always keep up-to-date store credentials. It's common to make mistakes with not that credentials;
     - Manage all response nodes: `isValid`, `duplicate` and `persisted`.