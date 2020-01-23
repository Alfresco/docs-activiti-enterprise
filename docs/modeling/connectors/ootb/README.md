---
Title: OOTB connectors
---

# OOTB connectors
Out of the box (OOTB) connectors are a set of pre-defined connectors that have been developed and are available to insert into service tasks within the Modeling Application. The available connectors are: 

* An [Aspose connector](../ootb/aspose.md) to generate files from data and variables using Aspose templates.
* A [Camel connector](../ootb/camel.md) to invoke Camel routes from a process.
* A [Comprehend connector](../ootb/comprehend.md) using Amazon's Comprehend service to extract insights and relationships from text.
* A [Docusign connector](../ootb/docusign.md) to send a document to an email address so that it can be signed and stored in Alfresco Content Services.
* A [DBP connector](../ootb/dbp.md) to create, update and delete content in an instance of Alfresco Content Services (ACS) as part of a process.
* An [email connector](../ootb/email.md) to automatically send emails as part of a process.
* A [Lambda connector](../ootb/lambda.md) to invoke AWS Lambda functions from a process.
* A [Rekognition connector](../ootb/rekognition.md) using Amazon's Rekognition service to identify and label objects from images. 
* A [REST connector](../ootb/rest.md) to connect a process to an external REST service.
* A [Salesforce connector](../ootb/salesforce.md) to undertake specific functions within an instance of Salesforce from a process.
* A [Slack connector](../ootb/slack.md) to create channels and send messages to an instance of Slack from a process.
* A [Textract connector](../ootb/textract.md) using Amazon's Textract service to extract text from images.
* A [Twilio connector](../ootb/twilio.md) to use Twilio to send SMS messages as part of a process.

## Configuring an OOTB connector within a project
OOTB connectors can be selected directly from the palette rather than by selecting a standard service task, though the input and output parameters need to be configured to process variables as with a standard service task.

**In the processes section of the Modeling Application**:

1. Select your OOTB connector from the connector palette. 
2. Choose whether to create a new instance of the connector or use an existing one.
 
	**Note**: Multiple actions linked to different [service tasks](../../processes/bpmn/service.md) can use the same instance of a connector image. Different instances of 	a connector are only required if you need to use different [connector variables](../../connectors/README.md/#connector-variables) for each.

3. Choose the `Action` your connector will use for that service task. 

	**Note**: Details of the actions are described for each OOTB connector in their respective 	pages.

4. Assign the input and output parameters for the connector to [process variables](../../processes/variables.md) within the process definition. If the names are identical this matching is done automatically.
5. Name the service task and complete your process definition model. 
6. Save the process definition. 

## Deploying OOTB connectors
The location of the images for OOTB connectors are predefined within the Administrator Application. When [deploying an application](../../../administrator/deploy/README.md) select the relevant connector(s) from the dropdown menu and set the connector variables as appropriate for your deployment. Details of the connector variables are described for each OOTB connector in their respective pages.
