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
	
	The tokens will need to be set as [connector variables](#connector-variables) when 	deploying the Slack connector.

## Connector variables
Environment variables that are specific to a connector need to be specified during deployment. They are entered as connector variables and used as environment variables for the connector when it is deployed. 

The following are the properties that need to be set for the Slack connector: 

| Variable | Description |
| -------- | ----------- |
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

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `pattern` | A regular expression to match messages and select which are published as events | String | Yes | 
| `echo` | The message sent to the user if a message is matched | String | No | 
| `echoError` | The message sent to the user if an error occurs publishing the event | 
| `channelTypes` | A list of channel types through which the message can be received, for example `public`, `mention`, `direct-message` | Array | No

### Output parameters

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `matchGroups` |  | Array | No |
| `originalMessage` | The original message that was sent | String | No |
| `slackChannelId` | The Slack channel ID of where the message was sent | String | No |
| `slackUserId` | The Slack user ID of the user that sent the mesesage | String | No |