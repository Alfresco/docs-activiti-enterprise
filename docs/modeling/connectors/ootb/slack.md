---
Title: Slack connector
---

# Slack connector
The Slack connector is used to integrate with the [Slack](https://slack.com) web API and REST time messaging API to create Slack channels and send messages to channels or users. The Slack connector is graphically represented by the Slack logo under the OOTB Connectors menu whilst modeling a process. 

The following actions can be executed using the Slack connector: 

* [Send a message](#send-message) to a specific user or channel (public or private)
* [Create a new channel](#create-channel) (public or private) 
    
## Slack configuration
The Slack connector requires a Slack application and a Slack bot in order to function. The application and bot need to be configured correctly. 

1. Use the [Slack website](https://api.slack.com/apps) to create an application. 

	**Note**: You will need to be logged in as a workspace administrator to create an 	application.
	
2. Use the following URL to create a bot in the application you created: 

	`https://api.slack.com/apps/<app_id>/bots`

3. Use the following URL to configure the scope and permissions of the application and bot: 

	`https://api.slack.com/apps/<app_id>/oauth`

	The following are the required [scope and permissions](https://api.slack.com/scopes): 

	* bot
	* channels:read
	* channels:write
	* groups:read
	* groups:write
	* mpim:read
	* mpim:write
	* users:read
	* users:read.email
	* chat:write:bot
	* chat:write:user
	* im:read
	* im:write 

4. Use the following URL to obtain the Slack bot user and admin tokens:

	`https://api.slack.com/apps/<app_id>/oauth`
	
	The tokens will need to be set as [configuration parameters](#configuration-parameters) for the connector.

## Configuration parameters
Values for configuration parameters that are specific to a connector instance can be set in the modeling application or during application deployment.

The following are the configuration parameters that need to be set for the Slack connector: 

| Parameter | Description |
| --------- | ----------- |
| `SLACK_XOXB` | The Slack bot user token obtained from [configuring](#slack-configuration) Slack beginning *xoxb-* |
| `SLACK_XOXP` | The Slack bot admin user token obtained from [configuring](#slack-configuration) Slack beginning *xoxp-* |

## Actions
The Slack connector contains two actions: 

* [`SEND_MESSAGE`](#send-a-message) that can send a message to a specific Slack channel.
* [`CREATE_CHANNEL`](#create-a-channel) that can create a Slack channel and invite members to join it.

### Send message
The `SEND_MESSAGE` action is used to send messages to either a Slack channel or a specific user. 

The `implementation` value of the send message action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_3xcd7zp" implementation="slackConnector.SEND_MESSAGE" />
```

#### Input parameters
The following are the parameters that can be passed to the Slack connector as input parameters using the `SEND_MESSAGE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `text` | The content of the message | String | Yes |
| `userId` | The user ID of the message recipient | String | * One of these fields is required |
| `userEmail` | The email address of the message recipient | String | * One of these fields is required |
| `channelId` | The channel ID to send the message to | String | * One of these fields is required |
| `channelName` | The name of the channel to send the message to | String | * One of these fields is required |
| `requestResponse` | <ul><li> Set to `no` a response will be sent back to the process immediately after a message is sent </li> <li> Set to `any` a response will be sent back to the process only after a reply is received in the same channel </li> <li> Set to `thread` a response will be sent back to the process only after a reply is received in a thread </li></ul> | String | No |

#### Output parameters
The following are the parameters that are returned to the process by the Slack connector as output parameters using the `SEND_MESSAGE` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `message` | If the input parameter `requestResponse` was set to `any` or `thread` then the content of the reply will be sent in this parameter | String |
| `slackError` | If an error was encountered it will be described in this parameter | String | 

### Create channel
The `CREATE_CHANNEL` action is used to create a new Slack channel. 

The `implementation` value of the create channel action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_7lhj7xq" implementation="slackConnector.CREATE_CHANNEL" />
```

#### Input parameters
The following are the parameters that can be passed to the Slack connector as input parameters using the `CREATE_CHANNEL` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `channelName` | The name of the channel to be created | String | Yes |
| `channelType` | Set whether the channel is `public` or `private` | String | Yes |
| `members` | A list of members that will be invited to join the new channel in the format: `["USER1","USER2","USER3"]` | String | Yes |

#### Output parameters
The following are the parameters that are returned to the process by the Slack connector as output parameters using the `CREATE_CHANNEL` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `slackResult` | If the channel is created successfully this will output the details of the channel | JSON object |
| `slackError` | If an error was encountered it will be described in this parameter. If the error was during channel creation, the process will stop. If the error was during the member invites then the process will continue | String |

An example of a successful `slackResult` is the following:

```json
{
  "ok":true,
  "channel":{
    "id":"CFWSKMFR6",
    "name":"my_channel",
    "is_channel":true,
    "created":1549985348,
    "is_archived":false,
    "is_general":false,
    "unlinked":0,
    "creator":"UFX13DBJM",
    "name_normalized":"my_channel",
    "is_shared":false,
    "is_org_shared":false,
    "is_member":true,
    "is_private":false,
    "is_mpim":false,
    "last_read":"0000000000.000000",
    "latest":null,
    "unread_count":0,
    "unread_count_display":0,
    "members":[
      "UFX13DBJM",
      "DFWSKM0HH"
    ],
    "topic":{
      "value":"",
      "creator":"",
      "last_set":0
    },
    "purpose":{
      "value":"",
      "creator":"",
      "last_set":0
    },
    "previous_names":[],
    "priority":0
  }
}
```

## Events
The Slack connector contains an event called `MESSAGE_RECEIVED` that can be used by a [trigger](../../modeling/triggers.md) to configure an action when a message meeting a specific pattern is found from a list of Slack channel types.

### Input parameters

| Parameter | Description | Example | Required? |
| --------  | ----------- | ------- | --------- |
| `pattern` | A regular expression that selects which messages are published as events. Java catching group syntax can be used to create groups from the pattern as variables  | Order Number `(?<orderNumber>.+)` | Yes | 
| `echo` | The message sent to the channel if a message is matched | Your reference number is `${orderNumber}` | No | 
| `echoError` | The message sent to the channel if an error occurs publishing the event | There was a problem publishing that event. | No | 
| `channelTypes` | A list of channel types through which the message can be received | public | No |

**Note**: Any groups created in a `pattern` can be referenced in `echo` and `echoError` using the syntax `${groupName}`.

### Output parameters

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `matchGroups` | Any matching groups found using the regular expression in `pattern` | JSON | No |
| `originalMessage` | The original message that was sent | String | No |
| `slackChannelId` | The Slack channel ID of where the message was sent | String | No |
| `slackUserId` | The Slack user ID of the user that sent the mesesage | String | No |

**Note**: Groups found in `matchGroups` can be used to map to process variables in a [trigger](../../../modeling/triggers.md) by referencing the variable in a JSON field, for example using `${matchGroups.orderNumber}`