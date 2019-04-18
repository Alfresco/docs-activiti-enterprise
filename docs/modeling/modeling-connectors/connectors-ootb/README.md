---
Title: OOTB connectors
---

# OOTB connectors
Out of the box (OOTB) connectors are a set of pre-defined connectors that have been developed and are available to insert into service tasks within the Modeling Application. The available connectors are: 

* An [email connector](../connectors-ootb/connectors-email.md) to automatically send emails as part of a process.
* A [REST connector](../connectors-ootb/connectors-rest.md) to connect a process to an external REST service.
* A [Lambda connector](../connectors-ootb/connectors-lambda.md) to invoke AWS Lambda functions from a process.
* A [Camel connector](../connectors-ootb/connectors-camel.md) to invoke Camel routes from a process.
* A [Salesforce connector](../connectors-ootb/connectors-salesforce.md) to undertake specific functions within an instance of Salesforce from a process.
* A [Slack connector](../connectors-ootb/connectors-slack.md) to create channels and send messages to an instance of Slack from a process.
* A [Twilio connector](../connectors-ootb/connectors-twilio.md) to use Twilio to send SMS messages as part of a process.

## Configuring an OOTB connector within a project
OOTB connectors can be selected directly from the palette rather than by selecting a standard service task, though the input and output parameters need to be configured to process variables as with a standard service task.

**In the processes section of the Modeling Application**:

1. Select your OOTB connector from the connector palette. 
2. Choose whether to create a new instance of the connector or use an existing one.
 
	**Note**: Multiple actions linked to different [service tasks](../../modeling-processes/	processes-bpmn/bpmn-service.md) can use the same connector instance. Different instances of 	a connector are only needed if you need to use different [connector variables](../../	modeling-connectors/README.md/#connector-definitions) for each.

3. Choose the `Action` your connector will use for that service task. 

	**Note**: Details of the actions are described for each OOTB connector in their respective 	pages.

4. Name the service task and complete your process definition model. 
5. Save the process definition. 

## Deploying OOTB connectors
The location of the images for OOTB connectors are predefined within the Administrator Application. When [deploying an application](../../../administrator/admin-deploy.md) select the relevant connector(s) from the dropdown menu and set the connector variables as appropriate for your deployment. Details of the connector variables are described for each OOTB connector in their respective pages.