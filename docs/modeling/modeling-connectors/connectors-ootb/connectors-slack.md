---
Title: Slack connector
---

# Slack connector
The Slack connector is used to integrate with the Slack web API and REST time messaging API. The following operations can be executed using the Slack connector: 

* Send a message to a specific user or channel (public or private)
* Create a new channel (public or private) 
    
## Configuration
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
	
	The tokens will need to be set when deploying the Slack connector:
	
	```
	slack.connector.bot.user.token=${SLACK_XOXB:xoxb-}
	slack.connector.admin.user.token=${SLACK_XOXP:xoxp-}
	```

## Send a message

There are different targets when sending a message to Slack. The following table describes the necessary parameters for each use case:

| Inbound Variable | Mandatory | Type | Description |
|---|---|---| --- |
| text  | YES | String | This variable is common to every request to the slack connector to set the text content when sending a message. | Using environment variable _SLACK_CONNECTOR_TEXT_KEY_, the value used as key can be configured. The default value is: _text_ |
| userId | NO* | String | This variable is used to set the target user in a direct message to slack using the user identifier. |
| userEmail | NO* | String | This variable is used to set the target user in a direct message to slack using the email user. |
| channelId | NO* | String | This variable is used to set the target channel when senting a message to slack using the channel identifier. |
| channelName | NO* | String | This variable is used to set the target channel when senting a message to slack using the channel name. |
| requestResponse | NO | Enum (no, any, thread) | Indicates whether the connector should return a response to APS immediately after sending the message (value "no"), wait until the user sends a reply in the same channel (value "any") or wait until the user sends a reply in a thread (value "thread") | 

*At least one out of channelId, userId, userEmail or channelName is required by the connector to identify which channel is the message being sent to.

> Channel identifiers can be extracted from Slack application getting last string in the link (e.g. https://development-urs7320.slack.com/messages/GFZFJ0UNQ, the channel ID is **GFZFJ0UNQ**) or using [slack rest api](https://api.slack.com/methods).

### Required configuration
As part of a BPMN definition, any Service Task responsible for sending a message to Slack needs to set **slackConnector.SEND_MESSAGE** as the value for its implementation attribute.


### Examples

#### Business Process Model
This is the [business process model](slack-business-process.bpmn20.xml) defined to test the different use cases when sending a message.

![Business Process Model to test Slack connector](slack_business_process.png)


Using this business process and using [postman collection](https://github.com/Activiti/activiti-cloud-examples/blob/develop/Activiti%20v7%20REST%20API.postman_collection.json) operations, the different cases can be tested.

> Prerequisites: It is necessary to have a valid keycloack token. This token can be retrieved using operation getKeyCloakToken into postman collection.

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
