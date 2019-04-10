---
Title: Slack connector
---

# Slack connector
The Slack connector is used to integrate with the Slack web API and REST time messaging API. The Slack connector is graphically represented by the Slack logo under the OOTB Connectors menu whilst modeling a process. 

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
	
	The tokens will need to be set as [connector variables](#connector-variables) when deploying the Slack 	connector.

## Connector variables
Environment variables that are specific to a connector need to be specified during deployment. They are entered as connector variables and used as environment variables for the connector when it is deployed. 

The following are the properties that need to be set for the Slack connector: 

| Variable | Description |
| -------- | ----------- |
| `SLACK_XOXB` | The Slack bot user token obtained from [configuring](#slack-configuration) Slack beginning *xoxb-* |
| `SLACK_XOXP` | The Slack bot admin user token obtained from [configuring](#slack-configuration) Slack beginning *xoxp-* |

## Send message
The `SEND_MESSAGE` action is used to send messages to either a channel or user. 

The `implementation` value of the send message action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_3xcd7zp" implementation="slackConnector.SEND_MESSAGE" />
```

### Input parameters
The following are the parameters that can be passed to the Slack connector as input parameters using the `SEND_MESSAGE` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `text` | The content of the message | `String` | Yes |
| `userId` | The user ID of the message recipient | `String` | * One of these fields is required |
| `userEmail` | The email address of the message recipient | `String` | * One of these fields is required |
| `channelId` | The channel ID to send the message to | `String` | * One of these fields is required |
| `channelName` | The name of the channel to send the message to | `String` | * One of these fields is required |
| `requestResponse` | <ul><li> Set to `no` a response will be sent back to the process immediately after a message is sent </li> <li> Set to `any` a response will be sent back to the process only after a reply is received in the same channel </li> <li> Set to `thread` a response will be sent back to the process only after a reply is received in a thread </li></ul> | `String` | No |

### Output parameters
The following are the parameters that are returned to the process by the Slack connector as output parameters using the `SEND_MESSAGE` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `message` | If the input parameter `requestResponse` was set to `any` or `thread` then the content of the reply will be sent in this parameter | `String` |
| `slackError` | If an error was encountered it will be described in this parameter | `String` | 

## Create channel
The `CREATE_CHANNEL` action is used to create a new Slack channel. 

The `implementation` value of the create channel action in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_7lhj7xq" implementation="slackConnector.CREATE_CHANNEL" />
```

### Input parameters
The following are the parameters that can be passed to the Slack connector as input parameters using the `CREATE_CHANNEL` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `channelName` | The name of the channel to be created | `String` | Yes |
| `channelType` | Set whether the channel is `public` or `private` | `String` | Yes |
| `members` | A list of members that will be invited to join the new channel in the format: `["USER1","USER2","USER3"]` | `String` | Yes |

### Output parameters
The following are the parameters that are returned to the process by the Slack connector as output parameters using the `CREATE_CHANNEL` action:

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `slackResult` ||
| `slackError` ||

















#### Send direct message

To send a direct message to a slack user, it is required to define _text_ and one of _user identifer_ or _user email_ as mandatory inbound variable. The process will try to search by user id, if it is the input param or by email if the Service Task sends that value as input parameter. If any of these parameters are not defined, an error will be returned.

##### Using user identifier
Using startProcess entry in the postman collection, a new process is created using the following body trying to send a slack message to the user id _ABC13DEFG_.

```
{
  "processDefinitionId": "SimpleProcess:1:4",
  "variables": {
    "text": "This is a direct message using user identifier.",
    "userId": "ABC13DEFG"
  },
  "payloadType":"StartProcessPayload"
}
```

##### Using user email

Using startProcess entry in the postman collection, a new process is created using the following body trying to send a slack message to the user email _user@test.com_.

```
{
  "processDefinitionId": "SimpleProcess:1:4",
  "variables": {
    "text": "This is a direct message using user email.",
    "userEmail": "user@test.com"
  },
  "payloadType":"StartProcessPayload"
}
```

#### Send Slack message to a channel (Public or Private)

To send a message to a Slack channel, it is required to define _text_ and one of _channel identifier_ or _channel name_ as mandatory inbound variable. The process will try to send the Slack message to the _channel id_ or to the _channel name_. If channel name is used, the process will try to search within public channels the name and if it does not exist, it will try to use within private ones. If any of these parameters are not defined, an error will be returned.

##### Using channel identifier
Using startProcess entry in the postman collection, a new process is created using the following body trying to send a slack message to the channel with id email _ABC13DEFG_.
```
{
  "processDefinitionId": "SimpleProcess:1:4",
  "variables": {
    "text": "This is a message to Slack channel identified by channel id.",
    "channelId": "ABC13DEFG"
  },
  "payloadType":"StartProcessPayload"
}
```

##### Using channel name
Using startProcess entry in the postman collection, a new process is created using the following body trying to send a slack message to the channel with id email _my_channel_.
```
{
  "processDefinitionId": "SimpleProcess:1:4",
  "variables": {
    "text": "This is a message to Slack channel identified by channel name.",
    "channelName": "my_channel"
  },
  "payloadType":"StartProcessPayload"
}
```

## Create a Slack Channel

### Configuration

As part of a BPMN definition, any Service Task responsible for sending a message to Slack needs to set **slackConnector.SEND_MESSAGE** as the value for its implementation attribute.


Parameters required to create an Slack Channel are the described in the following table.

| Inbound Variable | Mandatory | Description | Configuration |
| --- | --- | --- | --- |
| channelName | YES | Name which will be used when creating the channel | Using environment variable _SLACK_CONNECTOR_CHANNEL_NAME_KEY_, the value used as key can be configured. The default value will be _channelName_ |
| channelType | YES | Type of channel that will be created. It only can take 2 values: public or private. | Using environment variable _SLACK_CONNECTOR_CHANNEL_TYPE_KEY_, the value used as key can be configured. The default value will be _channelType_ |
| members | YES | List of members which will be invited to the created channel. The format will be `["USER1","USER2",...,"USERN"]` | Using environment variable _SLACK_CONNECTOR_CHANNEL_MEMBERS_KEY_, the value used as key can be configured. The default value will be _members_ |

> NOTE: To test this functionality it is necessary to define a business process model like this one [business process model](slack-create-channel.bpmn20.xml). As can be checked there is a ServiceTask which implementation field has been set with value **slackConnector.CREATE_CHANNEL**.
> The graphic defining the business process model looks lik the following one.
> ![Business Process Model to test Slack connector](slack_business_process.png) 

### Creating Public Slack Channel

#### Request
Using startProcess entry in the postman collection, a new process is created using the following body trying to create a Slack channel with name _my_channel_ and inviting an uses with email _user1@test.com_.
```
{
  "processDefinitionId": "SimpleProcess:1:4",
  "variables": {
    "channelName": "my_channel",
    "channelType": "public",
    "members": ["user1@test.com"]
  },
  "payloadType":"StartProcessPayload"
}
```
#### Response
Once the request has been launched, the result can take different ways.

| Outbound Variable |  Description | Example |
| --- | --- |  --- |
| slackResult | It contains the response with channel information after creating and inviting requested users | `{`<br/>&nbsp;&nbsp;`"ok":true,`<br/>&nbsp;&nbsp;`"channel":{`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"id":"CFWSKMFR6",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"name":"my_channel",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_channel":true,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"created":1549985348,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_archived":false,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_general":false,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"unlinked":0,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"creator":"UFX13DBJM",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"name_normalized":"my_channel",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_shared":false,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_org_shared":false,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_member":true,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_private":false,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_mpim":false,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"last_read":"0000000000.000000",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"latest":null,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"unread_count":0,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"unread_count_display":0,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"members":[`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"UFX13DBJM",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"DFWSKM0HH"`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`],`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"topic":{`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"value":"",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"creator":"",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"last_set":0`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`},`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"purpose":{`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"value":"",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"creator":"",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"last_set":0`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`},`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"previous_names":[],`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"priority":0`<br/>&nbsp;&nbsp;`}`<br/>`}` |
| slackError | It contains the error which happened during creating Slack channel operation. Error can be creating the channel (It stops the process) or inviting an user (It does not stop the process) | Error example message creating the channel:<br/><br/> _Error 'name_taken' happened while creating 'public' slack channel with name 'test_channel'_<br/><br/>Error example message creating the channel:<br/><br/>_Failed Users: [User 'UFX13DBJM' could not be added to channel 'test_channel' due to error 'not_found']_|

### Creating Private Slack Channel
Using startProcess entry in the postman collection, a new process is created using the following body trying to create a Slack channel with name _my_channel_ and inviting an uses with email _user1@test.com_.
```
{
  "processDefinitionId": "SimpleProcess:1:4",
  "variables": {
    "channelName": "my_channel",
    "channelType": "private",
    "members": ["user1@test.com"]
  },
  "payloadType":"StartProcessPayload"
}
```
#### Response
Once the request has been launched, the result can take different ways.

| Outbound Variable |  Description | Example |
| --- | --- |  --- |
| slackResult | It contains the response with channel information after creating and inviting requested users |  `{`<br/>&nbsp;&nbsp;`"ok": true,`<br/>&nbsp;&nbsp;`"group": {`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"id": "GG4SY55HR",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"name": "private_channel_test",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_group": true,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"created": 1549993306,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"creator": "UFX13DBJM",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_archived": false,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"name_normalized": "private_channel_test",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_mpim": false,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"is_open": true,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"last_read": "1549993306.000200",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"latest": {`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"user": "UFX13DBJM",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"type": "message",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"subtype": "group_join",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"ts": "1549993306.000200",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"text": "<@UFX13DBJM> has joined the group"`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`},`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"unread_count": 0,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"unread_count_display": 0,`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"members": [`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"UFX13DBJM",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"DFWSKM0HH"`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`],`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"topic": {`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"value": "",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"creator": "",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"last_set": 0`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`},`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"purpose": {`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"value": "",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"creator": "",`<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`"last_set": 0`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`},`<br/>&nbsp;&nbsp;&nbsp;&nbsp;`"priority": 0`<br/>&nbsp;&nbsp;`}`<br/>`}`<br/> |
| slackError | It contains the error which happened during creating Slack channel operation. Error can be creating the channel (It stops the process) or inviting an user (It does not stop the process) | Error example message creating the channel:<br/><br/> _Error 'name_taken' happened while creating 'private' slack channel with name 'test_channel'_<br/><br/>Error example message creating the channel:<br/><br/>_Failed Users: [User 'UFX13DBJM' could not be added to channel 'test_channel' due to error 'not_found']_|


## Instantiate an APS process when a Slack message is received

This connector allow users to configure different actions when a messages comes in. Set the property `aps.cloud.connector.config.inbound-message.actions` or the ENV variable INBOUND_MESSAGE_ACTION to a JSON value like this:

```
[
    {
        "event-types": [
            "direct-message", "mention", "private-channel"
        ],
        "pattern": "[Tt]ag ([\\\\w\\\\-]+) as ([\\\\w\\\\-]+)",
        "variables": {
            "fileName": "/files/$1",
            "tag": "$2"
        },
        "processKey": "process-ea538e1f-8466-41bf-a105-dc921fc16001",
        "echo": "Tagging your file $1 as $2",
        "echoError": "Error tagging the file."
    }
]
```

Further information about the JSON value format can be found [here](../alfresco-process-config-inbound-message).

APS Slack connector defines the following event types in order of precedence: 

| Event Type | Description |
|---| --- |
|direct-message| The message is sent directly from the user to APS Bot, whether it mentions the bot or not|
|mention| The message is sent to a public or private channel the bot is member of, and it contains a mention to the bot|
|private-channel| The message is sent to a private channel the bot is member of, and it does not mention the bot |
|public-channel| The message is sent to a public channel the bot is member of, and it does not mention the bot|

> NOTE: The connector will not receive any message sent to a public or private channel of which the APS Bot is not a member.
