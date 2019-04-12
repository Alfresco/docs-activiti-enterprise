---
Title: OOTB connectors
---

# OOTB connectors
Out of the box (OOTB) connectors are a set of pre-defined connectors that have been developed and are available to insert into service tasks within the Modeling Application. The available connectors are: 

* An [email connector]() to automatically send emails as part of a process
* A [REST connector]() to connect a process to an external REST service
* A [Lambda connector]() to invoke AWS Lambda functions from a process
* Camel
* Salesforce
* A [Slack connector]() to create channels and send messages to an instance of Slack from a process
* A [Twilio connector]() to use Twilio to send SMS messages as part of a process

## Configuring an OOTB connector within a project
OOTB connectors can be selected directly from the palette rather than by selecting a standard service task, though the input and output parameters need to be configured to process variables as with a standard service task.

**In the process section of the Modeling Application**:

* instances 

Details of the input and output parameters for each OOTB connector action are described in their respective documentation. 

## Deploying OOTB connectors
The location of the images for OOTB connectors are predefined within the Administrator Application. When [deploying an application](../../../administrator/admin-deploy.md) select the relevant connector(s) from the dropdown menu. 

* variables