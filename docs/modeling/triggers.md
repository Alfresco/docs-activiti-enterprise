---
Title: Triggers
---

# Triggers
Triggers listen for events and execute an action when the event criteria are met. 

Examples of what triggers can be used for include: 

* Starting a process instance when an email with a specific subject line is received.
* Sending a signal to a process when a specific message is written to a Slack channel. 

## Modeling triggers
Triggers contain an [event](#events) that is configured in the source component such as in a connector and an [action](#actions) configured for the individual trigger. 

Triggers are stored as JSON files in the format `<trigger-name>.json` and can be modeled using the GUI or the **JSON Editor**.

The basic properties for triggers are: 

| Property | Description | Example | 
| -------- | ----------- | ------- | 
| `ID` | 	The unique identifier for a trigger. This is system generated and cannot be altered. | trigger-223f6953-008a-426b-b22f-e8dd171ae47e | 
| `Name` | The name of the trigger. Trigger names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically. | email-approval-trigger |
| `Description` | A free text description of what the trigger does | A trigger to start a process when an approval email is received. | 

### Events
Events are what a trigger is configured to listen for and publish when the event criteria are met. 

The properties for trigger events are: 

| Property | Description | Example | 
| -------- | ----------- | ------- | 
| `Connector` | The [connector](../modeling/connectors/README.md) instance to use for an event | email-connector-1 | 
| `Event` | The [connector event](../modeling/connectors/README.md#events) to use for this trigger | EMAIL_RECEIVED | 
| `Input values` | The inputs for the trigger event. The properties are set in the [connector event](../modeling/connectors/README.md#events) definition | | 

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

The [email connector](../modeling/connectors/ootb/email.md), [Slack connector](../modeling/connectors/ootb/slack.md) and [Twilio connector](../modeling/connectors/ootb/twilio.md) have events defined that can be used for triggers.

### Actions
Actions are what the trigger executes when an event is met. The actions that can be configured are:

* [Start a process](#start-a-process)
* [Send a signal](#send-a-signal)

#### Start a process
The action to start a process will begin a specific process instance when a trigger event is met. The payload for the start a process action is:

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- |
| `processDefinitionKey` | The ID of the process to start | Process_YjqRw3n2 | Yes |
| `name` | The name of the process instance | approval-process | Yes | 
| `businessKey` | An optional business key ID for the process instance | 14240 | No | 
| `variables` | Values from the trigger can be mapped to [process variables](../modeling/processes/variables.md). For a connector this will include the output parameters configured in the [connector event](../modeling/connectors/README.md#events) definition. | | No | 

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
The action to send a signal will send a named [signal](../modeling/processes/bpmn/signal.md) of global scope when a trigger event is met. The payload for a send signal action is: 

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- |
| `Name` | The name of the signal being emitted. There must be a [signal catching event](../modeling/processes/bpmn/signal.md) with the same name for the payload to be received. | Signal_0n91cib | Yes | 
| `variables` | Values from the trigger can be mapped to [process variables](../modeling/processes/variables.md). For a connector this will include the output parameters configured in the [connector event](../modeling/connectors/README.md#events) definition. As a signal can be caught by multiple catching events, the variables must be written in JSON format. | | No | 

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
		"name": "Signal_0n91cib",

	}
}
```