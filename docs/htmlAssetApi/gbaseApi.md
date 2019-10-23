<a name="GbaseApi"></a>

## GbaseApi
**Kind**: global class  

* [GbaseApi](#GbaseApi)
    * [new GbaseApi(projectName, projectEnvironment, hmacSecret, platform, semVersion, _overrideUrlAddress)](#new_GbaseApi_new)
    * [.userInputBeenDone()](#GbaseApi+userInputBeenDone)
    * [.cloudFunction(functionName, argumentsToTransfer, callback)](#GbaseApi+cloudFunction)

<a name="new_GbaseApi_new"></a>

### new GbaseApi(projectName, projectEnvironment, hmacSecret, platform, semVersion, _overrideUrlAddress)
La constructor. Every instance of this class holds it's own instance of NetworkManager with
corresponding sequence, unicorn and sign up stuff.


| Param | Description |
| --- | --- |
| projectName | your project name like {abbreviated company name}-{abbreviated app name} |
| projectEnvironment | it's common practice to use any of these three: dev, qa or production |
| hmacSecret | a key for connection. Used for signing all messages. All platforms has different secrets. Also they can change between versions. You can get it by contacting us |
| platform | can see all available platforms using GbaseApi.PLATFORM; |
| semVersion | semantic version of current client build |
| _overrideUrlAddress | you can use it to specially override address. But usually you won't have to |

<a name="GbaseApi+userInputBeenDone"></a>

### gbaseApi.userInputBeenDone()
A very important method. You should call it once any of user input happen. Gbase sessions are tend to rot hence
API client configured to ping it once a while but it will stop after some time if no user input been done. So do call.

**Kind**: instance method of [<code>GbaseApi</code>](#GbaseApi)  
<a name="GbaseApi+cloudFunction"></a>

### gbaseApi.cloudFunction(functionName, argumentsToTransfer, callback)
Call a defined cloud function. It's a special guys that allows you to run domain logic on backend side - an authoritarian logic.

**Kind**: instance method of [<code>GbaseApi</code>](#GbaseApi)  

| Param | Description |
| --- | --- |
| functionName | string |
| argumentsToTransfer | any object |
| callback | standard callback(GbaseError err, GbaseResponse response) |