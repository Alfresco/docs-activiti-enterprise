---
Title: Twilio connector
---

# Twilio connector
The Twilio connector is used to integrate with an instance of [Twilio](https://twilio.com) to send SMS messages. The Twilio connector is graphically represented by the Twilio logo under the OOTB Connectors menu whilst modeling a process. 

The `implementation` value of the email connector in a service task would be similar to the following:

```xml
<bpmn2:serviceTask id="ServiceTask_1che7zm" implementation="emailConnector.SEND" />
```

## Twilio configuration
An account ID and token are required by the connector to access Twilio. These are specific to your Twilio account and act as the authorization credentials. 

The values are provided are provided by Twilio when an account is created. They can also be located in the [Twilio setup page](https://www.twilio.com/console/project/settings).

The account ID and authorization token will need to be set as [connector variables](#connector-variables) when deploying the Twilio connector.

## Connector variables
Environment variables that are specific to a connector need to be specified during deployment. They are entered as connector variables and used as environment variables for the connector when it is deployed. 

The following are the properties that need to be set for the Slack connector: 

| Variable | Description |
| -------- | ----------- |
| `TWILIO_ACCOUNT` | Your account name obtained from Twilio |
| `TWILIO_TOKEN` | A token for your account obtained from Twilio |

## Actions
The Twilio connector contains an action called `send_sms` that sends an SMS message using an external Twilio service. 

### Input parameters
The following are the parameters that can be passed to the Twilio connector as input parameters using the `send_sms` action:

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `smsFrom` | The phone number the SMS will be sent from | String | Yes |
| `smsTo` | The list of numbers the SMS will be sent to | String | Yes |
| `smsBody` | The SMS message to be sent | String | Yes |

**Note**: Freemarker templates are supported in `smsBody`. 

### Output parameters
The following are the parameters that are returned to the process by the Twilio connector as output parameters using the `send_sms` action:

**Note**: The execution of the Twilio connector is always successful. Any errors will be returned in the `twilioError` parameter. The parameter will be null if there were no errors.

| Parameter | Description | Type |
| --------  | ----------- | ---- |
| `twilioResult` | The response(s) from the connector containing the ID of the sent message | String |
| `twilioError` | A list of errors if any are caught by the connector | String |


## Events
The Twilio connector contains an event called `SMS_RECEIVED` that can be used by a [trigger](../../modeling/triggers.md) to configure an action when an SMS message meeting a specific pattern is found.

### Input parameters

| Parameter | Description | Example | Required? |
| --------  | ----------- | ------- | --------- |
| `pattern` | A regular expression that selects which messages are published as events. Java catching group syntax can be used to create groups from the pattern as variables  | Order Number `(?<orderNumber>.+)` | Yes | 
| `echo` | The message sent to the original sender if a message is matched | Your reference number is `${orderNumber}` | No | 
| `echoError` | The message sent to the user if an error occurs publishing the event | There was a problem publishing that event. | No | 

**Note**: Any groups created in a `pattern` can be referenced in `echo` and `echoError` using the syntax `${groupName}`

### Output parameters

| Parameter | Description | Type | Required? |
| --------  | ----------- | ---- | --------- |
| `matchGroups` | Any matching groups found using the regular expression in `pattern` | JSON | No |
| `originalMessage` | The original message that was sent | String | No |
| `to` | The recipient of the matched message | String | No |
| `from` | The sender of the matched message | String | No |

**Note**: Groups found in `matchGroups` can be used to map to process variables in a [trigger](../../../modeling/triggers.md) by referencing the variable in a JSON field, for example using `${matchGroups.orderNumber}`