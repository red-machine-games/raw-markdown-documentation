# Getting started 🚀
All products of Goblin Tech stack are open-source based - it means that they all are available at public repositories with full related functionality. You can find it at [Github](https://github.com/red-machine-games). In further sections, we'll overview how to get started with represented products.

## Table of contents
 - [Getting started with Goblin Base Server](#getting-started-with-goblin-base-server)
    - [Instantiate](#instantiate)
    - [Configurate](#configurate)
    - [More configurations](#more-configurations)
    - [Configure non mandatory features](#configure-non-mandatory-features)
    - [Add platforms and HMAC secrets](#ddd-platforms-and-hmac-secrets)
    - [Require first cloud function](#require-first-cloud-function)
    - [Running main web app only or pvp room only](#running-main-webapp-only-or-pvp-room-only)
 - [Full list of GoblinBase methods](#full-list-of-goblinbase-methods)
    - [includeAccounts([opts])](#includeaccountsopts)
    - [includeProfiles([opts])](#includeprofilesopts)
    - [includeWorkloadControl([opts])](#includeworkloadcontrolopts)
    - [hookLogs([opts])](#hooklogsopts)
    - [hookCloudFunctionsLogs([opts])](#hookcloudfunctionslogsopts)
    - [configureDatabase([opts])](#configuredatabaseopts)
    - [configureRedis([redisConfig])](#configureredisredisconfig)
    - [includeMatchmaking([opts])](#includematchmakingopts)
    - [includeAuthoritarian([opts])](#includeauthoritarianopts)
    - [includeLeaderboards([opts])](#includeleaderboardsopts)
    - [includeTickets([opts])](#includeticketsopts)
    - [includeCloudFunctions([opts])](#includecloudfunctionsopts)
    - [includeSimplePve([opts])](#includesimplepveopts)
    - [includePvp([opts])](#includepvpopts)
    - [includeMobileReceiptValidation([opts])](#includemobilereceiptvalidationopts)
    - [includeVkInappValidation([inappItems])](#includevkInappvalidationinappitems)
    - [includeOkInappValidation([inappItems])](#includeokInappvalidationinappitems)
    - [includeStatsdOutput([opts])](#includestatsdoutputopts)
    - [extendCloudFunctions(methodName, methodItself)](#extendcloudfunctionsmethodname-methoditself)
    - [requireAsCloudFunction(thePath)](#requireascloudfunctionthepath)
    - [addFacebookCredentials([opts])](#addfacebookcredentialsopts)
    - [addGooglePlayCredentials(googlePlayCredentials)](#addgoogleplaycredentialsgoogleplaycredentials)
    - [addAppStoreCredentials(opts)](#addappstorecredentialsopts)
    - [addVkCredentials(opts)](#addvkcredentialsopts)
    - [addOkCredentials(opts)](#addokcredentialsopts)
    - [addPlatform([opts])](#addplatformopts)
    - [enableNodeCors([opts])](#enablenodecorsopts)
    - [dontRunMainWebapp()](#dontrunmainwebapp)
 - [Getting started with client-side SDKs](#getting-started-with-client-side-sdks)
     - [Javascript SDK](#javascript-sdk)
        - [If installed from _npm_](#if-installed-from-npm)
        - [If inserted into _HTML_](#if-inserted-into-html)
        - [Full API documentation](#full-api-documentation)
     - [Start working](#start-working)
        - [Signup an anonymous player(user)](#signup-an-anonymous-player-user)
        - [Notes about session lifetime](#notes-about-session-lifetime)
        - [Notes about AFK](#notes-about-afk)
        - [Notes about PvP](#notes-about-pvp)
 - [Getting started with Goblin Base Benchmark](#getting-started-with-goblin-base-benchmark)
    - [Run benchmark on bootstrap project](#run-benchmark-on-bootstrap-project)
    - [An API of Goblin Base Benchmark](#an-api-of-goblin-base-benchmark)
        - [benchmark.Ns](#benchmarkns-array)
        - [benchmark.http(configs)](#benchmarkhttpconfigs-thehttpbench)
        - [benchmark.ws(configs)](#benchmarkwsconfigs-thewsbench)
        - [async benchmark.bottleneck(uniqueHead)](#async-benchmarkbottleneckuniquehead)
        - [async benchmark.shareValue(tabName, theKey, theValue)](#async-benchmarksharevaluetabname-thekey-thevalue)
        - [async benchmark.shareMany(tabName, keysAndValues)](#async-benchmarksharemanytabname-keysandvalues)
        - [async benchmark.getShare(tabName, theKey)](#async-benchmarkgetsharetabname-thekey)
        - [async benchmark.incrementAndGetShare(tabName, theKey)](#async-benchmarkincrementandgetsharetabname-thekey)
        - [async benchmark.unshare(tabName, theKey)](#async-benchmarkunsharetabname-thekey)
        - [async benchmark.listenShareEqualTo(tabName, theKey, preferableValue)](#async-benchmarklistenshareequaltotabname-thekey-preferablevalue)
        - [async TheHttpBench.prototype.getDone(onRequest, overrideAddressLambda)](#async-thehttpbenchprototypegetdoneonrequest-overrideaddresslambda)
        - [async TheWsBench.prototype.wsConnect(auxHead, queryLambda, overrideAddressLambda, defaultOnMessageListener)](#async-thewsbenchprototypewsconnectauxhead-querylambda-overrideaddresslambda-defaultonmessagelistener-wsbenchapi)
        - [WsBenchApi.prototype.onMessage(onMessageCallback)](#wsbenchapiprototypeonmessageonmessagecallback)
        - [WsBenchApi.prototype.onClose(onCloseCallback)](#wsbenchapiprototypeoncloseonclosecallback)
        - [async WsBenchApi.prototype.sendMessages(messageLambda, noLockingBefore)](#async-wsbenchapiprototypesendmessagesmessagelambda-nolockingbefore)
        - [async WsBenchApi.prototype.closeConnections(beforeCloseLambda)](#async-wsbenchapiprototypecloseconnectionsbeforecloselambda)
        - [async WsBenchApi.prototype.startMeasure(measureHead, n)](#async-wsbenchapiprototypestartmeasuremeasurehead-n)
        - [async WsBenchApi.prototype.stopMeasure(measureHead, n)](#async-wsbenchapiprototypestopmeasuremeasurehead-n)
        - [async WsBenchApi.prototype.pushMeasure(measureHead, n, measuredDuration)](#async-wsbenchapiprototypepushmeasuremeasurehead-n-measuredduration)

## Getting started with Goblin Base Server
The project itself is designed as a module that you just download from `NPM` and require into your code. Important things to notice: it doesn't use any logging lib, configuration delivery, and process management - it gives the user freedom to pick the most appropriate and beloved libs. The [Bootstrap repository](https://github.com/red-machine-games/goblin-base-server-bootstrap) represents the simple project that implements the server's lib with default configs - it's a great starting point for the developer. It hooks logs on the console, also there is `pm2process.json` file so you can just run it with [pm2](https://www.npmjs.com/package/pm2) process manager (btw `pm2` writes console logs into files - just type `$ pm2 logs` after installation, config and run).

### Instantiate
Server's API represented by `GoblinBase` class instance - it's a singleton yet hence don't use constructor.
```javascript
var GoblinBase = require('goblin-base-server'),
    goblinBase = GoblinBase.getGoblinBase();
```

### Configurate
First of all, let's configure mandatory things. The configuration process is done with chain-calls of `GoblinBase`'s instance

(We suppose that you already installed [Redis](https://redis.io/) and [MongoDB](https://www.mongodb.com/) and they are available at default localhost)
```javascript
// ...
goblinBase
    .hookLogs({ info: console.log, warn: console.log, error: console.error, fatal: console.error })
    .hookCloudFunctionsLogs({ info: console.log, warn: console.log, error: console.error, fatal: console.error })
    .configureDatabase({ connectionUrl: `mongodb://localhost:27017/hello-world-database` })
    .configureRedis(new GoblinBase.RedisConfig()
        .setupSessionsClient('localhost', 6379, { db: 0 })
        .setupLeaderboardClient('localhost', 6379, { db: 1 })
        .setupMatchmakingClient('localhost', 6379, { db: 2 })
        .setupPvpRoomClient('localhost', 6379, { db: 3 })
        .setupSimpleGameplayClient('localhost', 6379, { db: 4 })
        .setupServiceClient('localhost', 6379, { db: 5 })
        .setupMaintenanceClient('localhost', 6379, { db: 6 })
        .setupResourceLockerClient('localhost', 6379, { db: 7 })
    )
```
Here we firstly hooked logs: `hookLogs` is for main logs stream - it logs all system-related messages and errors, `hookCloudFunctionsLogs` is for logs inside of cloud functions. Redis and MongoDB config goes second - as you mentioned we have 7 Redis connections separated by domain logic and 1 separate PvP room Redis. Check out the [Scalability](https://github.com/red-machine-games/goblin-base-server#scalability) section to find out how it looks like.

### More configurations
Next let's configure mandatory features of Goblin Base Server.
```javascript
// ...
goblinBase
    .includeAccounts({ sessionLifetime: 60 * 1000, unicornSalt: 'sUpErSaLt' })
    .includeProfiles()
```

Here we configure the mandatory features. As we can see they have some options that will be fully listed below.


### Configure non mandatory features
All listed features including PvE and PvP are counted as non mandatory. So let's configure them
```javascript
// ...
goblinBase
    .includeTickets()
    .includeLeaderboards()
    .includeMatchmaking()
    //.includeAuthoritarian()
    .includePvp({
        apiPrefix: 'api/v0/',
        physicalHost: '127.0.0.1',
        physicalPort: 7332,
        displayPortWs: 7331,
        displayPortWss: 7333,
        shareIPAddress: true,
        bindUdpOnPort: 54321,
        pairsCapacity: 50,
        attachMessageTimeAtRoom: true
    })
    .includeSimplePve()
    .includeCloudFunctions()
```

Here we have all features aboard (_Grouping and chats WIP_). All chain methods can get an opts argument or default opts if no opt fiend or entire opt argument provided.

### Add platforms and HMAC secrets
Goblin Base Server protects communication with _foolproof_: all messages must be provided with a request sequence number and unique HMAC sign. At least it can protect your game or app from analyzing and faking requests from _Chrome dev console_.
```javascript
// ...
goblinBase
    .addPlatform({
        header: GoblinBase.PLATFORMS.STANDALONE,
        minimumVersion: '0.0.0',
        hmacSecretsMap: { '0.0.0': 'SUPER SECRET STRING THAT YOU SHOULD HIDE INSIDE OF YOUR CLIENT-SIDE BUILD' }
    });
```

So the purpose is simple: it controls the platform header(technically the Server doesn't care about origin platform of requests) and version header. You should keep the `minimumVersion` parameter up-to-date to block clients with out-of-date client builds - it responds with `code 400 body { index: 404, message: 'Go update!' }`. The `hmacSecretsMap` arguments represent different hmac secrets for different versions(the rule is _more or equal_) - if you suspect that your client build was hacked & secret was exposed, you can generate a new one and insert it into future version (all versions should semantic: `number.number.number`).

### Require first cloud function
Let's develop our first cloud function - it should increment some number value in a player's profile each time being called.
```javascript
await lock.self();

var thatCounter = await getProfileNode('profileData.thatCounter');

thatCounter = (thatCounter || 0) + 1;
setProfileNode('profileData.thatCounter', thatCounter);

FunctionResponse({ hereItIs: thatCounter });
```

Suppose that we put it into the `cloudFunction` directory with the file name `myFirstCloudFunction.js`. Now let's require it and start the server. The require mechanism works similar to _Node_'s.

```javascript
// ...
goblinBase
    .requireAsCloudFunction(`./cloudFunctions/myFirstCloudFunction.js`)
    .start(1337, 'localhost');
```

As a bonus: let's see how to run this cloud function with [JS SDK](https://github.com/red-machine-games/goblin-javascript-asset) within Node.js environment.
```javascript
var GbaseApi = require('gbase-html5-sdk').Gbase.GbaseApi,
    gbaseApiStdl = new GbaseApi(null, null, 'SUPER SECRET... blah-blah-blah', 'stdl', '0.0.2', 'http://localhost:1337');
    
await gbaseApiStdl.account.signupGbaseAnon();
await gbaseApiStdl.profile.create();
var thatCounter = (await gbaseApiStdl.cloudFunction('myFirstCloudFunction')).details.originalResponse.hereItIs;
```

Piece of cake.

### Running main web app only or pvp room only
Goblin Base Server, in fact, represents two servers at once:

 1. _Main web app server_ - with all profiles, leaderboards, cloud function, etc. features - HTTP only;
 2. _Pvp room server_ - receiving connections with `booking key` - HTTP and WebSockets.

To run first one - pass `port` and `host` arguments into `goblinBase.start(1337, 'localhost')`, call `dontRunMainWebapp()` chain method to skip the main web app server. To run the second - just include pvp: `goblinBase.includePvp({ ... })` and vice versa.

Configure database access as usual because all cluster members communicate with it, but there are some nuances with Redis connection: _main web app_ server needs only 7 Redis clients - except `PvpRoomClient`, but _pvp room_ server needs 2 Redis clients - `MatchmakingClient` because it's a connecting link between all cluster members, and `PvpRoomClient`. Check out the scalability schematic picture to get pick up the idea: [goblin-base-server#scalability](https://github.com/red-machine-games/goblin-base-server#scalability)

 > Note: Use [reverse proxy](https://nginx.org/) for all servers

## Full list of GoblinBase methods

### .includeAccounts([opts])
 - `opts` {Object}
    - `sessionLifetime` {Number} - Time to live of session in milliseconds without any requests. _default: 50 * 1000_ 
    - `lastActionTimeout` {Number} - Time in milliseconds how long client can ping session from last actual action. _default: 60 * 60 * 1000_
    - `unicornSalt` {String} - A salt used to generate new session key(unicorn). _default: "K9nesgpvPSb44VN6mx35Vn9Q"_
    - `gClientIdSalt` {String} - A salt used to generate gClientId. _default: "cg7DqfvjLuk6BusZZzwbSjVA"_
    - `gClientSecretSalt` {String} - A salt used to generate gClientSecret. _default: "cMZQMsDRM84bbRSprDrvWKV5"_

Configures accounts and auth functionality - mandatory.

### .includeProfiles([opts])
 - `opts` {Object}
    - `numericConstants` {Object}
        - `unlinkedProfileTtlMs` {Number} - System property, ttl of new profile failed to link with account accidentally. _default: 1000 * 60 * 10_
        - `profilesRefreshBatchSize` {Number} - System property, amount of profiles sample for backgrouns jobs. _default: 50_
        - `profilesRefreshPackageTimeout` {Number} - System property, time milliseconds between background job samples. _default: 1000 * 60_
        - `profilesRefreshAllTimeout` {Number} - System property, time milliseconds after background finish before next start. _default: 1000 * 60 * 30_

Configures profiles functionality - mandatory.

### .includeWorkloadControl([opts])
 - `opts` {Object}
    - `eventLoopMaxLag` {Number} - Maximum event loop lag time after which server responds with { index: 592, message: 'Too busy now!' }. _default: 5 * 1000_
    - `eventLoopCheckInterval` - Event loop check interval in milliseconds. _default: 500_
    - `artificiallyLimitCCU` - Limit number of simultaneous sessions on backend. Useful while development. 0 is to disable. _default: 0_

Configures workload-related things.

### .hookLogs([opts])
 - `opts` {Object}
    - `info` {Function} It'll be called to log _info_ level. _default: NOOP_
    - `warn` {Function} It'll be called to log _warn_ level. _default: NOOP_
    - `error` {Function} It'll be called to log _error_ level. _default: NOOP_
    - `fatal` {Function} It'll be called to log _fatal_ level. _default: NOOP_

It hooks the logs output. You can provide any function like _console_'s log or implement full featured logging module.

### .hookCloudFunctionsLogs([opts])
 - `opts` {Object}
    - `info` {Function} It'll be called to log cloud function run _info_ level. _default: NOOP_
    - `warn` {Function} It'll be called to log cloud function run _warn_ level. _default: NOOP_
    - `error` {Function} It'll be called to log cloud function run _error_ level. _default: NOOP_
    - `fatal` {Function} It'll be called to log cloud function run _fatal_ level. _default: NOOP_

It hooks only cloud functions logs. Find out more how to log cloud functions here: https://gbase.tech/doc/api/debuggingCloudFunctions/

### .configureDatabase([opts])
 - `opts` {Object}
    - `connectionUrl` {String} - MongoDB [connection url](https://docs.mongodb.com/manual/reference/connection-string/). _default: "mongodb://localhost:27017/dev"_
    - `autoIndex` {Boolean} - Build indexes provided by Goblin Base Server. _default: true_
    - `poolSize` {Number} - The maximum size of the individual server pool. _default: 5_
    - `writeConcern` {Number} - A MongoDB's [write concern](https://docs.mongodb.com/manual/reference/write-concern/#w-option). _default: 1_
    - `journalConcern` {Boolean} - A MongoDB's [journal concern](https://docs.mongodb.com/manual/reference/write-concern/#j-option). _default: true_
    - `wtimeout` {Number} - The write concern timeout. _default: 15 * 1000_
    - `devNewDocValidation` {Boolean} - Whever validate documents when `createNew` method used. _default: true_

Configures MongoDB connection.

### .configureRedis([redisConfig])
 - `redisConfig` {lib/config/RedisConfig}
    - `.setupSessionsClient(host, port, opts)` {Function} - a method to setup _sessions_ client connection
        - `host` {String} - An ip address or domain name where Redis located
        - `port` {Number} - A port where Redis ran
        - `opts` {Object} - A [connection options](http://redis.js.org/#api-rediscreateclient)
    - `.setupMatchmakingClient(host, port, opts)` {Function} - a method to setup _matchmaking_ client connection
        - `host` {String} - An ip address or domain name where Redis located
        - `port` {Number} - A port where Redis ran
        - `opts` {Object} - A [connection options](http://redis.js.org/#api-rediscreateclient)
    - `.setupLeaderboardClient(host, port, opts)` {Function} - a method to setup _leaderboards_ client connection
        - `host` {String} - An ip address or domain name where Redis located
        - `port` {Number} - A port where Redis ran
        - `opts` {Object} - A [connection options](http://redis.js.org/#api-rediscreateclient)
    - `.setupSimpleGameplayClient(host, port, opts)` {Function} - a method to setup _simple PvE_ client connection
        - `host` {String} - An ip address or domain name where Redis located
        - `port` {Number} - A port where Redis ran
        - `opts` {Object} - A [connection options](http://redis.js.org/#api-rediscreateclient)
    - `.setupServiceClient(host, port, opts)` {Function} - a method to setup _service_ client connection
        - `host` {String} - An ip address or domain name where Redis located
        - `port` {Number} - A port where Redis ran
        - `opts` {Object} - A [connection options](http://redis.js.org/#api-rediscreateclient)
    - `.setupMaintenanceClient(host, port, opts)` {Function} - a method to setup _maintenance_ client connection
        - `host` {String} - An ip address or domain name where Redis located
        - `port` {Number} - A port where Redis ran
        - `opts` {Object} - A [connection options](http://redis.js.org/#api-rediscreateclient)
    - `.setupResourceLockerClient(host, port, opts)` {Function} - a method to setup _lock_ client connection
        - `host` {String} - An ip address or domain name where Redis located
        - `port` {Number} - A port where Redis ran
        - `opts` {Object} - A [connection options](http://redis.js.org/#api-rediscreateclient)
    - `.setupPvpRoomClient(host, port, opts)` {Function} - a method to setup _realtime pvp_ client connection(for pvp gameroom)
        - `host` {String} - An ip address or domain name where Redis located
        - `port` {Number} - A port where Redis ran
        - `opts` {Object} - A [connection options](http://redis.js.org/#api-rediscreateclient)

Configures Redis connections.

## .includeMatchmaking([opts])
 - `opts` {Object}
    - `maxSearchRanges` {Number} - Limit matchmakig request's or matchmaking call's from cloud function length of property `mmDetails.rgs`. _default: 4_
    - `rememberAsyncOpponentMs` {Number} - How long in milliseconds you will be unable to matchmake the same opponent as before. _default: 1000 * 60 * 5_
    - `strategy` {String} - Can be `open` - to allow matchmakig from request or from `onMatchmaking` cloud function with _by rating_ & _by ladder_ strategies and `mmDetails`, can be `predefined` - to disallow. _default: "open"_
    - `limitMmr` {Number} - A matchmaking tune for `predefined` strategy. _default: 0_
    - `limitLeaderboardRadius` {Number} - A matchmaking tune for `predefined` strategy. _default: 15_
    - `searchBothSides` {Boolean} - A matchmaking tune for `predefined` strategy. _default: true_
    - `bookingKeySalt` {String} - A salt to generate booking keys. _default: "7zJ3HxTCdNGacv3uQv3Aq5yHgN2DevFLsxFwpdzvhqrGAqYx"_
    - `numericConstants` {Object}
        - `longPollingColdResponseAfterMs` {Number} - After how many milliseconds responds with code 200 and body `{ stat: 'MM: searching', c: 0 }` on matchmaking. _default: 10 * 1000_
        - `longPollingDestroyAfterMs` {Number} - After how many milliseconds destroys long polling request. _default: 25 * 1000_
        - `timeForSearchMs` {Number} - How much milliseconds particular player can be in _searching_ state during whole matchmaking before server respond with `{ ... c: -1 }`. _default: 30 * 1000_
        - `timeForAcceptanceMs` {Number} - How much milliseconds particular player can be in _accepting_ state during whole matchmaking. _default: 30 * 1000_
        - `refreshStatsReloadingMs` {Number} - The reloading of running yet another background job refreshing matchmaking operative data(how frequently). _default: 1000_
        - `refreshStatsBatchSize` {Number} - How much entities background job works out each tick. _default: 100_
        - `gameroomBookingTtl` {Number} - For how many milliseconds server will hold booking key in operative database without any actions. _default: 5 * 60 * 1000_
        - `playerInGameroomTtl` {Number} - For how many milliseconds matchmaking module will remember that particular player is in pvp room currently without any actions. _default: 90 * 1000_

Configures matchmaking functionality.

### .includeAuthoritarian([opts])
 - `opts` {Object}
    - `disallowDirectProfileExposure` {Boolean} - Disallows directly read & modify all profile, leadererboards and battle journal data. _default: false_
    - `disallowDirectPvpMatchmakingExposure` {Boolean} - Disallows direct transfering `strategy` & `mmDetails` from request, instead runs a `onMatchmaking` cloud function. _default: false_
    - `disallowDirectChatAndGroupsExposure` Boolean - Disallows directly read & modify groups and chats. _default: false_

Configures authoritarian behavior.

### .includeLeaderboards([opts])
 - `opts` {Object}
    - `order` {String} - A listing order, can be _"asc"_ or _"desc"_. _default: "desc"_
    - `whitelistSegments` {Array<String>} - Whitelist of allowed to post into segments. _default: ["def"]_
    - `allowPublicListing` {Boolean} - Allowing getting overall leaders for [non-logged in users](https://gbase.tech/doc/api/leaderboard/#get-overall-leaderboard). _default: false_
    - `numericConstants` {Object}
        - `seedPageTime` {Number} - Time in milliseconds to seed one page into operative data. _default: 15 * 1000_
        - `batchSize` {Number} - A size of batch or page for any background jobs of leaderboards module. _default: 50_
        - `refreshPackageTimeout` {Number} - Time in milliseconds between refreshing batches of operative data background job. _default: 1000_
        - `allRefreshTimeout` {Number} - Time in milliseconds before new refreshing batches of operative data background job. _default: 60 * 1000_
        - `socFriendsCacheTtl` {Number} - For how long in milliseconds social friends cache will live. _default: 1000 * 60 * 60 * 24 * 7_
        - `operativeRecordLifetime` {Number} - For how long records in operative database will live without refreshing background job tick. _default: 1000 * 60 * 60 * 5_

Configures leaderboards functionality.

### .includeTickets([opts])
 - `opts` {Object}
    - `ticketLifetime` {Number} - Lifetime of ticket in milliseconds. _default: 1000 * 60 * 60 * 24 * 7_

Configures tickets functionality.

### .includeCloudFunctions([opts])
 - `opts` {Object}
    - `customSession` {Boolean} - Whether cloud function's `session` global variable included or not. _default: true_
    - `enableTraces` {Boolean} - Enable [traces API](https://gbase.tech/doc/api/debuggingCloudFunctions/#traces). _default: true_
    - `enableSetTimeout` {Boolean} - Enable native `setTimeout` inside of cloud function(works only for Goblin Cloud Server). _default: false_
    - `resources` {Object} - A map of any resources provided into cloud functions. _default: null_
    - `allowToPushInitContext` {Boolean} - Allow or disallow to push `initContext` cloud function. _default: false_
    - `atomic` {Object}
        - `batchSize` {Number} - How much atomic acts background job grabs at single tick. _default: 100_
        - `refreshPackageTimeout` {Number} - Time in milliseconds between each background job tick. _default: 1000_
        - `allRefreshTimeout` {Number} - Time in milliseconds between background job full cycles. _default: 10 * 1000_

Configures cloud functions and atomic acts.

### .includeSimplePve([opts])
 - `opts` {Object}
    - `pveBattleTimeout` {Number} - For how long in milliseconds simple pve battle lives without acts. _default: 1000 * 60 * 2_
    - `pveBattleDebtTimeout` {Number} - For how long in milliseconds battle debt(mechanism to detect a leaver) entry lives. _default: 1000 * 60 * 60 * 24 * 7_
    - `packJsonModel` {Boolean} - Whever pack gameplay model with [jsonpack](https://www.npmjs.com/package/jsonpack) - you can turn it on if no unexpected behavior met. _default: false_

Configures simple PvE functionality.

### .includePvp([opts])
 - `opts` {Object}
    - `physicalHost` {String} - Where to start _pvp room_'s server physically. _default: "127.0.0.1"_
    - `displayHost` {String} - A host value that goes to player after successful matchmaking at `.hosts.asDomain`
    - `shareIPAddress` {Boolean} - Adds actual external IP address value at `.hosts.asIP`. _default: false_
    - `displayPortWs` {Number} - A websocket port value goes to player after successful matchmaking at `.ports.ws`. _default: 80_
    - `displayPortWss` {Number} - A websocket port value goes to player after successful matchmaking at `.ports.wss`. _default: 443_
    - `physicalPort` {Number} - An which port _pvp room_'s server physically runs. _default: 7332_
    - `bindUdpOnPort` {Number} - On which port physically bind UDP listener. This port value goes at `.ports.dgram`. _default: 54321_
    - `apiPrefix` {Number} - (WIP). _default: "api/v0/"_
    - `pairsCapacity` {Number} - An amount of pvp pairs(1 pair = 2 players) that this particular pvp room will accept. _default: 100_
    - `resendFinalWsMessages` {Boolean} - Send closing message once before actual close. Useful in case if your client can't receive close message for some reason. _default: false_
    - `attachMessageTimeAtRoom` {Boolean} - Attack `_t` property to messages that represents processing time in milliseconds. _default: false_
    - `numericConstants` {Object}
        - `heartbeatIntervalMs` {Number} - A milliseconds interval of heartbeat - a function that checks all websocket connections and refreshes operative database. _default: 500_
        - `timeToConnectPairMs` {Number} - How many time in milliseconds opponents can spend on connecting and all preparations before pvp pair dies. _default: 15 * 1000_
        - `checkSocketsEveryMs` {Number} - An allowed interval of how frequently heartbeat function can check every particular websocket. _default: 2 * 1000_
        - `connectionLockTtlMs` {Number} - Technical lifetime of websocket connection's exclusive lock. _default: 15 * 1000_
        - `messageLockTtlMs` {Number} - Technical lifetime of http message's exclusive lock. _default: 5 * 1000_
        - `pairInGameTtlMs` {Number} - Technical lifetime of non-paused pair without actions & updates before it dies. _default: 30 * 1000_
        - `socketTtlMs` {Number} - Technical lifetime of websocket connection without pings & messages before it being terminated by heartbeat function. _default: 3 * 1000_
        - `timeToProcessMessageMs` {Number} - Technical time in milliseconds for how long one turn message can be processing by one particular Node.js instance(of single pvp room cluster). _default: 10 * 1000_
        - `unpausedGameTtlMs` {Number} - Technical value similar to `pairInGameTtlMs`. _default: 30 * 1000_
        - `pausedPairTtlMs` {Number} - Technical lifetime of paused pair without actions & updates before it dies. _default: 20 * 1000_
        - `pausedTimedoutPairInactivityMs` {Number} - Deprecated value. _default: 15 * 1000_
        - `refreshStatsBatchSize` {Number} - Amount of pairs processed per one tick of background pvp room job. _default: 100_
        - `refreshStatsReloadingMs` {Number} - Interval in ms for how frequently background pvp room job tick performed. _default: 1000_
        - `refreshOccupationReloadingMs` {Number} - Interval in ms for how frequently pvp room's occupation synced with matchmaking subsystem. _default: 1000_
        - `absoluteMaximumGameplayTtlMs` {Number} - Absolute lifetime of pair gameplay in milliseconds no matter what. _default: 15 * 60 * 1000_

Configures and runs pvp room's http/websocket server.

### .includeMobileReceiptValidation([opts])
 - `opts` {Object}
    - `imitate` {Object} - Don't communicate with store servers but imitate response instead: value must be `{ isValid: true }`, `{ isValid: false }` or `false`. _default: false_
    - `verbose` {Boolean} - Whether turn on in-app purchase validation lib's logs output. _default: false_
    - `forceSandbox` {Boolean} - For Apple and Googl Play to force Sandbox validation only. _default: false_
    - `appleExcludeOldTransactions` {Boolean} - If you want to exclude old transaction, set this to true. _default: false_

Configures in-app purchase validation for Google Play and App Store receipts.

### .includeVkInappValidation([inappItems])
 - `inappItems` {Array}
    - `type` : {lib/config/VkInappItem}
        - `itemId` {String} - Valid ID for particular in-app lot. Should be similar to artifact ID
        - `title` {String} - A player-oriented translated title
        - `photoUrl` {String} - A valid url pointing to image
        - `price` {Number} - A price for this particular lot

Configures VK.com in-app purchases. Configure VK.com app through it's admin panel, find out more how to provide service callback: https://gbase.tech/doc/api/socialNetwork/

### .includeOkInappValidation([inappItems])
 - `inappItems` {Array}
    - `type` : {lib/config/VkInappItem}
        - `productCode` {String} - Valid ID for particular in-app lot. Should be similar to artifact ID
        - `productOption` {Number} - Player-oriented valuable any number
        - `price` {Number} - A price for this particular lot

Configures OK.ru in-app purchases. Configure OK.ru app through it's admin panel, find out more how to provide service callback: https://gbase.tech/doc/api/socialNetwork/

### .includeStatsdOutput([opts])
 - `opts` {Object}
    - `statsdHost` {String} - A target host where to send metrics. _default: "127.0.0.1"_
    - `statsdPort` {Number} - A target port where to send metrics. _default: 8125_
    - `numericConstants` {Object}
        - `sessionsCounterBatchSize` {Number} - CCU grabbind background job batch size of single tick. _default: 100_
        - `sessionsCounterBlockMs` {Number} - How much time in milliseconds between CCU grabbind background job cycles. _default: 5 * 1000_
        - `sessionsCounterLazyBlockMs` {Number} - A node-local interval in milliseconds between CCU grabbind background job ticks. _default: 1000_

Configures StatsD output

### .extendCloudFunctions(methodName, methodItself)
 - `methodName` {String} - Declare an custom function that will be available from all cloud functions from global variable `extensionsAPI.*`
 - `methodItself` {Function} - A method of extension

### .requireAsCloudFunction(thePath)
 - `thePath` {String} - Requires target javascript file as cloud function. Works similar to native Node's `require` function

### .addFacebookCredentials([opts])
 - `opts` {Object}
    - `clientId` {Number} - An app's value from Facebook's admin panel
    - `clientSecret` {String} - An app's value from Facebook's admin panel
    - `useTokenAsId` {Boolean} - Turn on whether token will be interpreted as end user Facebook ID skipping request to Facebook. Appropriate for development purposes
    - `serviceApi` {Object}
        - `url` {String} - An URL of Facebook's service API. _default: "https://graph.facebook.com/"_
        - `externalApiTimeout` {Number} - Timeout of requests to Facebook's API. _default: 20 * 1000_
    - `markerApi` {Object}
        - `url` {String} - URL to get Facebook service marker. _default: `${DEFAULT_SERVICE_API_URL}oauth/access_token`_
        - `grandType` {URL} - A part of service marker request. _default: "client_credentials"_
        - `defaultMarkerLifetime` {Number} - A lifetime of service marker in milliseconds. _default: 86400 * 1000_

Adds Facebook credentials. Mandatory to use `WEBFB` platform and login with Facebook's token

### .addGooglePlayCredentials(googlePlayCredentials)
 - `googlePlayCredentials` {lib/config/GooglePlayCredentials}
    - `serviceAccount(clientEmail, privateKey, bundleId)` {Function}
        - `clientEmail` {String} - Client email from Google API service account JSON key file
        - `privateKey` {String} - Private key string from Google API service account JSON key file
        - `bundleId` {String} - A Bundle ID of your android project
    - `keys(sandboxPublicKey, livePublicKey, bundleId)` {Function}
        - `sandboxPublicKey` {String} - This is the google iap-sandbox public key string
        - `livePublicKey` {String} - This is the google iap-live public key string
        - `bundleId` {String} - A Bundle ID of your android project

Adds Google Play credentials. To include receipt validation it's mandatory to add Google Play and/or App Store credentials.

### .addAppStoreCredentials(opts)
 - `opts` {Object}
    - `applePassword` {String} - This comes from iTunes Connect (You need this to valiate subscriptions)
    - `bundleId` {String} - A Bundle ID of your iOS project

Adds App Store credentials. To include receipt validation it's mandatory to add Google Play and/or App Store credentials.

### .addVkCredentials(opts)
 - `opts` {Object}
    - `clientId` {Number} - VK.com's app ID can be found in admin panel
    - `clientSecret` {String} - VK.com's app secret can be found in admin panel
    - `useTokenAsId` {Boolean} - Turn on whether token will be interpreted as end user VK.com ID skipping request to VK.com. Appropriate for development purposes
    - `devHelp` {Boolean} - Turn on details to error response `{ index: 416, message: 'Wrong "auth_key"' }`. It helps to debug
    - `serviceToken` {Object}
        - `url` {String} - An url where to get VK.com's service token. _default: "https://oauth.vk.com/access_token"_
        - `grandType` {String} - A part of service token url request. _default: "client_credentials"_
        - `v` {String} - A version of service token API. _default: "5.87"_
        - `defaultTokenLifetime` {Number} - A lifetime of service marker in milliseconds. _default: 86400 * 1000_
        - `externalApiTimeout` {Number} - Timeout of requests to VK.com's API. _default: 20 * 1000_

Adds VK.com credentials. Mandatory to use `WEBVK` platform, login with VK.com's token, with `vkId` & `vkSecret` and process in-apps via service callback.

### .addOkCredentials(opts)
 - `opts` {Object}
    - `applicationPublicKey` {String} - OK.ru's app public key can be found in admin panel
    - `applicationSecretKey` {String} - OK.ru's app secret key can be ordered into app owner's email
    - `useTokenAsId` {Boolean} - Turn on whether token will be interpreted as end user OK.ru ID skipping request to OK.ru. Appropriate for development purposes
    - `devHelp` {Boolean} - Turn on details to error response `{ index: 436, message: 'Wrong social secret' }`. It helps to debug
    - `serviceApi` {Object}
        - `url` {String} - An url where to get OK.ru's service token. _default: "https://api.ok.ru/fb.do?"_
        - `externalApiTimeout` {Number} - Timeout of requests to OK.ru's API. _default: 20 * 1000_

Adds OK.ru credentials. Mandatory to use `WEBOK` platform, login with OK.ru's token, with `okId` & `okSessionKey` & 'okSecret' and process in-apps via service callback.

### .addPlatform([opts])
 - `opts` {Object}
    - `header` {String} - A header can be: `stdl`/`android`/`ios`/`webfb`/`webvk`/`webok`. _default: "stdl"_
    - `minimumVersion` {String} - Semantic version `number.number.number` of minimum available version of client. _default: "0.0.0"_
    - `hmacSecretsMap` {Object} - A map representing minimal versions and appropriate HMAC secrets. _default: `{ [DEFAULT_MINIMUM_VERSION]: 'default' }`_

Adds a platform. You must add platforms according to your client builds and hide HMAC secrets.

### .enableNodeCors([opts])
 - `opts` {Object}
    - `origin` {String} - Access-Control-Allow-Origin. _default: "*"_
    - `allowHeaders` {String} - Access-Control-Allow-Headers. _default: "X-Req-Seq,X-Request-Sign,X-Platform-Version,X-Unicorn,Accept,Content-Type,DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Content-Range,Range,X-Benchmark-Intervals,X-Book-Key"_
    - `exposeHeaders` {String} - Access-Control-Expose-Headers. _default: "X-Api-Version,X-Balance-Version,Accept,Content-Type,DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Content-Range,Range,Pragma"_

Enables Node.js CORS in case you didn't configure it on the reverse proxy.

### .dontRunMainWebapp()

Don't run _main web app_'s server. Then you must run _pvp room_'s server.

## Getting started with client-side SDKs

Client-side SDKs (assets) are designed to encapsulate as much mess as possible from the user. 2 SDKs are available at the moment: [javascript SKD](https://github.com/red-machine-games/goblin-javascript-asset) and [Unity C# asset](https://github.com/red-machine-games/goblin-unity3d-asset)

 > Note: Unity asset is on very WIP stage right now and doesn't meet the current vision of SDKs also slightly unready to be used. We working hard to make it meet the common vision of SDKs with all required features. Hence this getting started section will describe only the javascript SDK.
 
## Javascript SDK
 
 Can be used from Node.js or from browser. 2 ways on how to get it as a dependency:

 - Get it with _npm_: `$ npm install --save gbase-html5-sdk`
 - Insert it into _HTML_ code: `<script src="https://gbase-public-static.ams3.cdn.digitaloceanspaces.com/gbase-html5-latest-min.js"></script>`

Henceforth there is some nuance about instantiating API depending on the way you require it into the project:

### If installed from _npm_:
```javascript
var GbaseApi = require('gbase-html5-sdk').Gbase.GbaseApi;
// And objects...
```

### If inserted into _HTML_:
```javascript
// At this case "Gbase" global variable is declared with following objects:
Gbase.GbaseApi; // - Is an API class
Gbase.GbaseResponse;    // - Is an instance of API non-error response class
Gbase.GbaseError;   // - Is actually an error class of API instance
Gbase.GbaseRangePicker; // - Is a matchmaking range builder class
Gbase.GbasePvp; // - Is a class of formed PvP gameplay
```

### Full API documentation
Is hosted here: [![Latest Documentation](https://doxdox.org/images/badge-flat.svg)](https://doxdox.org/red-machine-games/goblin-javascript-asset)

!!! info "All api features are divided into few sections"
    - `#!js account`
    - `#!js profile`
    - `#!js leaderboards`
    - `#!js matchmaking`
    - `#!js tickets`
    - `#!js social`
    - `#!js pve`
    - `#!js pvp`
    - `#!js utils`

## Start working
Let's overview how to actually start using the API, we suppose that you already instantiated fresh Node.js project and installed SDK from _npm_.

### Signup an anonymous player(user)
```javascript
var GbaseApi = require('gbase-html5-sdk').Gbase.GbaseApi,
    gbaseApi;

// We will write async/await code for simplicity. Promise returned if no callback provided
(async () => {
    
    gbaseApi = new GbaseApi('mystudio-mygame', 'game', 'the_hmac_secret', '0.0.0', GbaseApi.PLATFORM.STDL); // If we use Goblin Cloud Server
    // or...
    gbaseApi = new GbaseApi('the_hmac_secret', '0.0.0', GbaseApi.PLATFORM.STDL, 'http://localhost:1337');   // If we use open sources Goblin Base Server
    
    GbaseApi.dropCache();   // Optional
    
    try{
        // Signin with gClientId & gClientSecret but here we omit them so they be taken from local storage
        await this.gbaseApi.account.signinGbase();
    } catch(__){
        // And we will catch an error if no credentials or appropriate player
        // It's okay for first visit so we just sign up. The server will generate credentials and automatically put it into local storage
        await this.gbaseApi.account.signupGbaseAnon();
    }
    // After signup/login you must create or get self profile
    if(!this.gbaseApi.currentAccount.haveProfile){
        await this.gbaseApi.profile.create();
    } else {
        await this.gbaseApi.profile.getp();
    }
    
    //  Henceforth we will use the API
    
}).then(() => console.log('Okay')).catch(err => console.error(err));
```

Notable detail: it uses [store](https://www.npmjs.com/package/store) npm package to automatically store credentials, call `GbaseApi.dropCache();` to clear it.

### Notes about session lifetime
Goblin's Server is session-based - it means that you get it on auth and then use it for all further things. So session lifetime is limited with `sessionLifetime` parameter (you configure it on `includeAccounts`) and after timeout you'll get an any of following error responses with code 401: `{ index: 423, message: 'Unknown unicorn' }` or `{ index: 891, message: 'Unknown unicorn' }`. It's okay so just login again with the `reauth` method and go further. Here is a rough example of handling:
```javascript
async getWorldRankAndScore(){
    try{
        let rankResp = await this.gbaseApi.leaderboards.getSelfRecord();
        // ...
    } catch(err){
        if(err.code === 68 || (err.details && err.details.originalStatus === 401)){
            await this.gbaseApi.account.reAuth();
            await this.gbaseApi.profile.getp();
            return await this.getWorldRankAndScore();
        }
    }
}
```

Checout out more [examples at Demo game](https://github.com/red-machine-games/clash-of-cats-game/blob/master/src/controllers/ControllerHowardResponsibleForNetworking.js)'s repository

### Notes about AFK
It's a common case when the player gets Away From Keyboard (Away From Komputer) - you should handle it rightly and let the session to die. To prevent session dead while player you should call a special method: `gbaseApi.userInputBeenDone();` - Goblin Base's API handles ping functionality internally but it should be sure that player actually doing something. You can call this method on any input or call it with an interval if you are definitely sure that the player is not AFK.

If the player(user) is AFK the session dies soon hence handle it as mentioned in prev. section. Also, you can use the `logout` event but it's not persistent due to dependency on `ping` request:
```javascript
gbaseApi.on('logout', () => console.log('You\'re AFK'));
```

### Notes about PvP
Here some notes that can be useful in building PvP functionality.

You can call a method `await gbaseApi.pvp.dropMatchmaking();` before actual matchmaking. It drops operative data about your current matchmaking state. It's useful in case your client build glitched, crashed, desynced with a backend or in case of any unexpected behavior. But if you'll drop the PvP match you're currently in - you get the auto defeat.

Pvp provides a commons: timestamp (UTC-0) of beginning and seed for [mersenne twister](https://en.wikipedia.org/wiki/Mersenne_Twister) - everything for you to be fully in sync with your opponent. But ping can break in and make a shift in time as far as the difference between your pings milliseconds is big enough. To dodge this problem you can calculate local time shift based on ping: make a few ping probes, get the median and the half is your shift. Here is an example from [examples at Demo game](https://github.com/red-machine-games/clash-of-cats-game/blob/master/src/controllers/ControllerHowardResponsibleForNetworking.js)'s repository:
```javascript
const TIME_DIFF_PROBES = 10;
// ...
async _measureTimeDiff(){
    if(this.gbaseApi.currentAccount){
        let _tdprobes = [];
        for(let i = 0 ; i < TIME_DIFF_PROBES ; i++){
            let _ts1 = Date.now(),
                _stime = await this.gbaseApi.utils.getServerTime(),
                _halfPing = Math.round((Date.now() - _ts1) / 2);
            if(!_stime.ok){
                throw new Error('Unsuccessful');
            }
            _tdprobes.push((_ts1 + _halfPing) - _stime.details.originalResponse);
        }
        this._tdiff = utils.getTheMedian(_tdprobes);
    }
}
```
 
Anyway check out full code of demo game as an example of API usage: [clash-of-cats-game](https://github.com/red-machine-games/clash-of-cats-game)

## Getting started with Goblin Base Benchmark
A scenario-based distributed benchmark tool made with languages javascript and Lua, based on Node.js and Redis, no native dependencies. It was made to simulate workloads for [Goblin Base Server](https://github.com/red-machine-games/goblin-base-server) but works fine with any http endpoints and websocket endpoints.

**Important note**: this benchmark is not that significantly optimized to work from a single instance - he's unlikely to show good numbers. If you plan to use a single machine benchmark - look at [Autocannon](https://www.npmjs.com/package/autocannon) or [Wrk](https://github.com/wg/wrk). Otherwise, if you aimed to expose exactly Goblin Benchmark's distributed nature and develop full-featured javascript scenarios - give it a try. Javascript scenario means literally plain javascript with all its features - it's convenient to use if you need to run some imposing client-side logic for requests: they can be some argument calculations or even full libraries of authoritarian logic (like gameplay library). Just tune cluster size and batch size to gain various numbers of requests per second and response time.

It's working principle looks like a cluster of Node.js instances running the same javascript file independently but before yet another HTTP burst or WebSockets burst, they all coordinated by Redis to start that burst at the same time, after that the same Redis coordinator collects the results. In the end, results are collected and calculated by single cluster members and dumped into files. To make scenario development easy all API methods are [async](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function).

### Run benchmark on bootstrap project
First, need to get the project and install appropriate software(node and databases). We are going to run it locally so we have to fix PvP configurations a little:
```javascript
.includePvp({
    apiPrefix: 'api/v0/',
    physicalHost: '127.0.0.1',
    displayHost: '127.0.0.1',
    physicalPort: 7332,
    // displayPortWs: 7331,
    // displayPortWss: 7333,
    displayPortWs: 7332,
    displayPortWss: 0,
    // shareIPAddress: true,
    shareIPAddress: false,
    // bindUdpOnPort: 54321,
    bindUdpOnPort: 0,
    pairsCapacity: 200,
    attachMessageTimeAtRoom: true
})
```

 > Note: zero ports mean no bind or no display

Next lets install the benchmark `$ npm install -g goblin-base-benchmark`, download [example preferences](https://github.com/red-machine-games/goblin-base-benchmark/blob/master/PREFS_EXAMPLES/goblinBaseServerPrefs.example.json) and [example scenario](https://github.com/red-machine-games/goblin-base-benchmark/blob/master/SCENARIO_EXAMPLES/benchmarkGoblinBaseServer.example.js), run the Server (_be sure that server ran on clean databases_) and run the benchmark: `$ goblin-base-benchmark --peers 200 --bulkSize 10 --onHost localhost --onPort 1337 --redisHost localhost --redisPort 6379 --proc 4 --pref "path/to/prefs.json" --bench "path/to/scenario.js"`. **Note**: if you see errors try to reduce `peers`, `proc` or `bulkSize` number. After benchmark you should see many `OK`s and something like this in your terminal:
```
...
0 | Work is done for #1!
1 | getMeasurements @ getBenchDone @ benchSubmitter.js...
1 | getMeasurements @ getBenchDone @ benchSubmitter.js... OK
1 | getTheResult @ getBenchDone @ benchSubmitter.js...
1 | getTheResult @ getBenchDone @ benchSubmitter.js... OK
1 | ensureResultsDir @ persistResults @ getBenchDone @ benchSubmitter.js...
1 | ensureResultsDir @ persistResults @ getBenchDone @ benchSubmitter.js... OK
1 | lockLocal @ persistResults @ getBenchDone @ benchSubmitter.js...
1 | lockLocal @ persistResults @ getBenchDone @ benchSubmitter.js... OK
1 | checkTheResultsFiles @ persistResults @ getBenchDone @ benchSubmitter.js...
2 | getMeasurements @ getBenchDone @ benchSubmitter.js...
2 | getMeasurements @ getBenchDone @ benchSubmitter.js... OK
1 | checkTheResultsFiles @ persistResults @ getBenchDone @ benchSubmitter.js... OK 2
2 | getTheResult @ getBenchDone @ benchSubmitter.js...
1 | unlockLocal @ persistResults @ getBenchDone @ benchSubmitter.js...
2 | getTheResult @ getBenchDone @ benchSubmitter.js... OK
1 | unlockLocal @ persistResults @ getBenchDone @ benchSubmitter.js... OK
1 | Work is done for #2!
2 | ensureResultsDir @ persistResults @ getBenchDone @ benchSubmitter.js...
2 | ensureResultsDir @ persistResults @ getBenchDone @ benchSubmitter.js... OK
2 | lockLocal @ persistResults @ getBenchDone @ benchSubmitter.js...
2 | lockLocal @ persistResults @ getBenchDone @ benchSubmitter.js... OK
2 | checkTheResultsFiles @ persistResults @ getBenchDone @ benchSubmitter.js...
2 | checkTheResultsFiles @ persistResults @ getBenchDone @ benchSubmitter.js... OK 2
3 | getMeasurements @ getBenchDone @ benchSubmitter.js...
2 | unlockLocal @ persistResults @ getBenchDone @ benchSubmitter.js...
3 | getMeasurements @ getBenchDone @ benchSubmitter.js... OK
3 | getTheResult @ getBenchDone @ benchSubmitter.js...
2 | unlockLocal @ persistResults @ getBenchDone @ benchSubmitter.js... OK
2 | Work is done for #3!
3 | getTheResult @ getBenchDone @ benchSubmitter.js... OK
3 | ensureResultsDir @ persistResults @ getBenchDone @ benchSubmitter.js...
3 | ensureResultsDir @ persistResults @ getBenchDone @ benchSubmitter.js... OK
3 | lockLocal @ persistResults @ getBenchDone @ benchSubmitter.js...
3 | lockLocal @ persistResults @ getBenchDone @ benchSubmitter.js... OK
3 | checkTheResultsFiles @ persistResults @ getBenchDone @ benchSubmitter.js...
3 | checkTheResultsFiles @ persistResults @ getBenchDone @ benchSubmitter.js... OK 2
3 | unlockLocal @ persistResults @ getBenchDone @ benchSubmitter.js...
3 | unlockLocal @ persistResults @ getBenchDone @ benchSubmitter.js... OK
3 | Work is done for #4!
```

It means that all 4 node independent processes end okay. Now you should find results at directory `_benchmarkResults_` of your `cwd` with many appropriate JSON files - for each request burst. Here is an example of a file:
```javascript
{
    "fileName": "builtin-accountAndProfile-createProfile",
    "brief": {
        "codes": {
            "200": 400
        },
        "infoByCodes": {
            "200": {
                "avg": 169.5225,
                "min": 99,
                "max": 350,
                "peakPercentage": 0
            }
        },
        "errors": 0,
        "RPS": {
            "min": 30,
            "max": 190,
            "median": 190,
            "avg": 133,
            "perEverySecond": [
                190,
                180,
                30
            ],
            "avgND2": 185,
            "perEverySecondND2": [
                190,
                180
            ]
        },
        "medianTime": 161
    },
    "numOfResults": 400,
    "benchmark": {}
}
```

 > Note: I run from old laptop so don't carp to numbers.
 
## An API of Goblin Base Benchmark
### benchmark.Ns : Array
Returns array of Ns - counting numbers of peers related to that exact Node.js instance. Example: if we have `--peers`=16 and `--proc`=4 then first instance will have [0, 1, 2, 3], second - [4, 5, 6, 7], third - [8, 9, 10, 11] and the last one - [12, 13, 14, 15].

### benchmark.http(configs) : TheHttpBench
 - `configs` {Object}
    - `method` {String} - An http method. _default: "get"_
    - `head` {String} - A header for this particular burst - represented at result json file
    - `uri` {String} - URI **Without Query variables**
    - `overridePrefix` {String} - Overrides URI prefix mentioned at prefs' `URI_PREFIX`
    - `noUnicorn` {Boolean} - Do not use Goblin Base Server's unicorn. Anyway won't work if prefs' `GOBLIN_BASE_HMAC_SECRET` and `GOBLIN_BASE_PLATFORM_VERSION` are not provided
    - `overrideTarget` {Object}
        - `host` {String} - A host to override `--onHost` argument
        - `port` {Number} - A port to override `--onPort` argument

Instantiates(it's not a run yet) an http burst(all peers attack) on exact URI.

### benchmark.ws(configs) : TheWsBench
 - `configs` {Object}
    - `head` {String} - A header for this particular burst - represented at result json file
    - `uri` {String} - URI **Without Query variables**
    - `overridePrefix` {String} - Overrides URI prefix mentioned at prefs' `URI_PREFIX`
    - `overrideTarget` {Object}
        - `host` {String} - A host to override `--onHost` argument
        - `port` {Number} - A port to override `--onPort` argument

Instantiates(it's not a run yet) a websocket burst(all peers attack) on exact URI.

### async benchmark.bottleneck(uniqueHead)
 - `uniqueHead` {String} - A unique head string for this particular bottleneck

Bottlenecks(Syncs) execution for all nodes in cluster at the place of call.

### async benchmark.shareValue(tabName, theKey, theValue)
 - `tabName` {String} - A tabname where to put
 - `theKey` {String} - A key where to put
 - `theValue` {String} - What to put

Shares the value into Redis. Henceforth the value is available for all nodes.

### async benchmark.shareMany(tabName, keysAndValues)
 - `tabName` {String} - A tabname where to put
 - `keysAndValues` {Object} - A map of keys and values

Shares many values into Redis at once. Unlike `shareValue` method shares all values with single request hence it can't fail at half of the way.

### async benchmark.getShare(tabName, theKey)
 - `tabName` {String} - A tabname where to get
 - `theKey` {String} - A key where to get

Retrieves share from Redis. It's a common cache for all nodes so everybody can get any data.

### async benchmark.incrementAndGetShare(tabName, theKey)
 - `tabName` {String} - A tabname where to get
 - `theKey` {String} - A key where to get

Like `getShare` method retrieves share additionally incrementing it by one on the way - atomic.

### async benchmark.unshare(tabName, theKey)
 - `tabName` {String} - A tabname where to get & remove
 - `theKey` {String} - A key where to get & remove

Retrieves value to but removes it right away - atomic.

### async benchmark.listenShareEqualTo(tabName, theKey, preferableValue)
 - `tabName` {String} - A tabname where to watch
 - `theKey` {String} - A key where to watch
 - `preferableValue` {String} - What value expected

Blocks the execution until watched value will not become `preferableValue`.

### async TheHttpBench.prototype.getDone(onRequest, overrideAddressLambda)
 - `onRequest` {Function} - A lambda that returns object like `{ query: { ... }, body: { ... }, headers: { ... } }`. Can be _async_
 - `overrideAddressLambda` {Function} - A lambda returns array like `[overrideHost, overridePort]`. Can be _async_

Runs instantiated http burst.

### async TheWsBench.prototype.wsConnect(auxHead, queryLambda, overrideAddressLambda, defaultOnMessageListener) : WsBenchApi
 - `auxHead` {String} - A postfix for file name with connections measures
 - `queryLambda` {Function} - A lambda returning connection query as an object. Can be _async_
 - `overrideAddressLambda` {Function} - A lambda returning override address: `[target host, target port]`. Can be _async_
 - `defaultOnMessageListener` {Function} - A lambda that fires on message but before benchmark's `onMessage` listener. Can be _async_

Runs instantiated websockte's connections burst. And returns instance to run the messages burst, etc.

### WsBenchApi.prototype.onMessage(onMessageCallback)
 - `onMessageCallback` {Function} - Actual `onMessage` event listener. Can be _async_

Listen for every message of every websocket.

### WsBenchApi.prototype.onClose(onCloseCallback)
 - `onCloseCallback` {Function} - Actual `onClose` event listener. Can be _async_

Listen for close of every websocket.

### async WsBenchApi.prototype.sendMessages(messageLambda, noLockingBefore)
 - `messageLambda` {Function} - A lambda that returns an object like `{ message: { ... }, bookingKey: ... }`(`bookingKey` is optional). Can be _async_
 - `noLockingBefore` {Boolean} - Do not make bottleneck for all nodes before actual sendings.

Broadcast through all websockets associated with particular benchmark.

### async WsBenchApi.prototype.closeConnections(beforeCloseLambda)
 - `beforeCloseLambda` {Function} - A lambda called before each websocket close with argument `n` - websocket's counting number

Close all websockets.

### async WsBenchApi.prototype.startMeasure(measureHead, n)
 - `measureHead` {String} - A unique header
 - `n` {Number} - Associated counting number

Start measure for peer `n`.

### async WsBenchApi.prototype.stopMeasure(measureHead, n)
 - `measureHead` {String} - A unique header
 - `n` {Number} - Associated counting number

End measure for peer `n`.

### async WsBenchApi.prototype.pushMeasure(measureHead, n, measuredDuration)
 - `measureHead` {String} - A unique header
 - `n` {Number} - Associated counting number
 - `measuredDuration` {Number} - Predefined value

Push predefined measure for peer `n`.