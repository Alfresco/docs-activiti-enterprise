---
Title: Triggers
---

# Triggers
Triggers listen to integrations and create an event when a set of defined event criteria are met. An action is then creates a payload from the event and sends it to a defined channel using [Rabbit MQ](../architecture/application.md#rabbit-mq).

Examples of what triggers can be used for include: 

* Starting a process instance when an email with a specific subject line is received.
* Sending a signal to a process when a specific message is written to a Slack channel.
* Sending a message when a file is created in Alfresco Content Services. 

## Modeling triggers
Triggers contain an [event](#events) definition that is configured in the source component such as a connector and an [action](#actions) configured for the individual trigger. 

Triggers are stored as JSON files in the format `<trigger-name>.json` and can be modeled using the GUI or the **JSON Editor**.

The basic properties for triggers are: 

| Property | Description | Example | 
| -------- | ----------- | ------- | 
| `ID` | 	The unique identifier for a trigger. This is system generated and cannot be altered. | trigger-223f6953-008a-426b-b22f-e8dd171ae47e | 
| `Name` | The name of the trigger. Trigger names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically. | email-approval-trigger |
| `Description` | A free text description of what the trigger does | A trigger to start a process when an approval email is received. | 

### Events
Events are created when the configured event criteria are met. 

The properties for trigger events are: 

| Property | Description | Example | 
| -------- | ----------- | ------- | 
| `source` | A combination of the source of the event, the name of the source and event name  to monitor in the format `<source-type>.<source-name>.<event-name>` | connector.my-email.EMAIL_RECEIVED | 
| `Input values` | The inputs for the trigger event. | | 

Events are stored as a `source` containing the connector and event to use and a set of `inputs` containing the values for the trigger event, for example using the `EMAIL_RECEIVED` event for the [email connector](../modeling/connectors/ootb/email.md):

```json
"event": {
	"inputs": {
		"pattern": "",
		"echo": "Email pattern matched. Starting a process instance.",
		"echoError": "We hit a problem starting a process instance for that message. "
	},
	"source": "connector.email-connector-1.EMAIL_RECEIVED"
}
```

The following are the events that can be configured for triggers:

* When a specific [application runtime bundle event](../architecture/application.md#application-runtime-bundle) occurs.
* When files and folders are created in Alfresco Content Services using the [content connector](../modeling/connectors/ootb/content.md#events).
* When an email is received using the [email connector](../modeling/connectors/ootb/email.md#events).
* When specific messages are received in a Slack channel using the [Slack connector](../modeling/connectors/ootb/slack.md#events).
* When an SMS message that meets a certain pattern is received using the [Twilio connector](../modeling/connectors/ootb/twilio.md#events).

### Actions
Actions are the trigger payload that is created and sent when an event is created. The payload for an action can include the output parameters configured for an event. 

The following are the actions that can be configured for triggers:

* Any [connector](../modeling/connectors/ootb.md) actions
* [Start a process](#start-a-process)
* [Send a signal](#send-a-signal)
* [Receive a message](#receive-a-message)
* [Send a start message](#send-a-start-message)
* [Execute a script](#execute-a-script)

#### Start a process
The action to start a process will begin a specific process instance when event are met. The payload for the start a process action is:

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- |
| `processDefinitionKey` | The ID of the process to start | Process_YjqRw3n2 | Yes |
| `name` | The name of the process instance | approval-process | Yes | 
| `businessKey` | An optional business key ID for the process instance | 14240 | No | 
| `variables` | Values from the trigger can be mapped to [process variables](../modeling/processes/variables.md). For a connector this will include the output parameters configured in the [connector event](../modeling/connectors/README.md#events) definition. | | No | 
| `source` | The source is automatically set and is the channel that the action is sent to | command-consumer | 

Actions are stored as a `payload`, for example using the start a process action for the `EMAIL_RECEIVED` event of the [email connector](../modeling/connectors/ootb/email.md):

```json
"action": {
	"source": "command-consumer",
	"payload": {
		"payloadType": "StartProcessPayload",
		"processDefinitionKey": "Process_YjqRw3n2",
		"variables": {
			"sender": "${emailFrom}",
			"recipient": "${emailTo}",
			"subject": "${emailSubject}"
		},
		"name": "approval-process",
		"businessKey": "14240"
	}
}
```

#### Send a signal
The action to send a signal will send a named [signal](../modeling/processes/bpmn/signal.md) of global scope when an event criteria are met. The payload for a send signal action is: 

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- |
| `Name` | The name of the signal being emitted. There must be a [signal catching event](../modeling/processes/bpmn/signal.md) with the same name for the payload to be received. | Signal_0n91cib | Yes | 
| `variables` | Values from the trigger can be mapped to [process variables](../modeling/processes/variables.md). For a connector this will include the output parameters configured in the [connector event](../modeling/connectors/README.md#events) definition. As a signal can be caught by multiple catching events, the variables must be written in JSON format. | | No | 
| `Source` | The source is automatically set and is the channel that the action is sent to | command-consumer | 

Actions are stored as a `payload`, for example using the send a signal action for the `EMAIL_RECEIVED` event of the [email connector](../modeling/connectors/ootb/email.md):

```json
"action": {
	"source": "command-consumer",
	"payload": {
		"payloadType": "StartProcessPayload",
		"variables": {
			"sender": "${emailFrom}",
			"recipient": "${emailTo}",
			"subject": "${emailSubject}"
		},
		"name": "Signal_0n91cib"
	}
}
```

#### Receive a message
The action to receive a message will send a named [message](../modeling/processes/bpmn/message.md) when an event criteria are met. The payload for a receive message action is:

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- |
| `Name` | The name of the message to send. There must be an [intermediate message catching event](../modeling/processes/bpmn/message.md#message-intermediate-catch-events) or [message boundary event](../modeling/processes/bpmn/boundary.md#message-boundary-events) with the same name for the payload to be received. | Message_077epax | Yes | 
| `correlationKey` | An optional [correlation key](../modeling/processes/bpmn/message.md#correlation-keys) can be provided for matching the message | 014-245 | No | 
| `payloadType` | The payload type is set automatically to `ReceiveMessagePayload`. | | Yes |
| `variables` | Values from the trigger can be mapped to [process variables](../modeling/processes/variables.md). For a connector this will include the output parameters configured in the [connector event](../modeling/connectors/README.md#events) definition. | | No | 
| `Source` | The source is automatically set and is the channel that the action is sent to | command-consumer | 

Actions are stored as a `payload`, for example using the receive a message action for the `EMAIL_RECEIVED` event of the [email connector](../modeling/connectors/ootb/email.md):

```json
"action": {
	"source": "command-consumer",
	"payload": {
		"payloadType": "ReceiveMessagePayload",
		"name": "Message_077epax",
		"correlationKey": "00-1245"
	}
}
```

#### Send a start message
The action to send a start message will send a named [message](../modeling/processes/bpmn/message.md) when an event criteria are met to start a process that contains a [start message event](../modeling/processes/bpmn/start.md#message-start-events). The payload for a send message action is:

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- |
| `Name` | The name of the message to send. There must be a [start message event](../modeling/processes/bpmn/start.md#message-start-events) with the same name for the payload to be received. | Message_077epax | Yes |  
| `businessKey` | An optional business key can be sent with the message to contain custom values. | | No | 
| `payloadType` | The payload type is set automatically to `StartMessagePayload`. | | Yes |
| `variables` | Values from the trigger can be mapped to [process variables](../modeling/processes/variables.md). For a connector this will include the output parameters configured in the [connector event](../modeling/connectors/README.md#events) definition. | | No | 
| `Source` | The source is automatically set and is the channel that the action is sent to | command-consumer | 

Actions are stored as a `payload`, for example using the send a start message action for the `EMAIL_RECEIVED` event of the [email connector](../modeling/connectors/ootb/email.md):

```json
"action": {
	"source": "command-consumer",
	"payload": {
		"payloadType": "StartMessagePayload",
		"name": "Message_077epax",
		"businessKey": "event-message",
		"variables": {
			"trigger": "${name}"
		}
	}
}
```

#### Execute a script
The action to execute a script will execute a named [script](../modeling/scripts.md) that exists in the same project as the trigger.

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- |
| `scriptName` | The name of the script is automatically populated from the selected script. | update-orders-script | Yes | 
| `scriptId` | The ID of the script is automatically populated from the selected script. | 19ced673-e701-4e6c-ace6-f8aaee5455eb | Yes | 
| `variables` | Values from the trigger event outputs can be mapped to script variables. | | No | 