---
Title: Connectors
---

# Connectors 
Connectors are used to import data into your process from an external source or system, or to update an external source or system using data from your process. They are also used to execute logic outside of a runtime bundle and pass the result(s) back to the process. Connectors are attached to [service tasks]() within a process definition.

Connectors conceptually contain two parts:

* An [image](#connector-images) of the connector that is deployed alongside a project. This image needs to contain the logic that the connector executes outside of the runtime bundle. 
* A [connector definition](#connector-definitions) that describes the actions a connector can make and the parameters that need to be passed to the connector image. The connector definition is used to attach a connector action to a service task within a process definition. 

Examples of what connectors can be used to action in a process include:

* automating emails using values from a process to populate the fields 
* interacting with an installation of Slack to post messages
* triggering a custom REST API and passing the response back to the process 

Connector components can also be [extended further](https://www.alfresco.com/abn/adf/docs/process-services-cloud/) using the Application Development Framework. 

## Connector images
To execute logic outside of a runtime bundle, a connector is deployed as a separate image. The communication between the runtime bundle and connector is accomplished via specifically named channels with messages managed by a message broker (Rabbit MQ by default). 

The environment variables that are specific to the connector can be set as connector variables during deployment.

## Connector definitions
Connector definitions can be created or updated through the GUI or using the inbuilt JSON editor. A connector must have a name and description followed by an unlimited number of actions that each contain input and output parameters of data type; boolean, date, integer or string.

A single connector can have multiple actions, for example creating a Slack channel and sending a message to a Slack channel are different actions but they can be handled by the same connector.

Within each action of a connector definition input and output parameters need to be defined. Input parameters are those that are sent from the process to the connector and output parameters are those that are sent back. Input and output parameters need to be paired to [process variables]() within a process definition to pass values for each parameter back and forth and optionally reuse those values later on in a process.

Once a connector definition has been created, they are attached using the `implementation` value of a [service task]() within a process definition using the format `<connector-name>.<action-name>`. The following is an example of the XML for a service task of a Slack connector executing the send message action: 

```xml
<bpmn2:serviceTask id="ServiceTask_3xcd7zp" implementation="slackConnector.SEND_MESSAGE" />
```

Each service task that a connector is attached to can only execute a single action, however the same connector can be used multiple times within the same process definition to execute the same action or different actions, as long as they are attached to different service tasks and the connector variables for the connector image are the same. If different connector variables are required then multiple connector definitions will need to be defined that are configured to different connector images.

## Creating connectors
A set of [OOTB (out of the box) connectors](../modeling-connectors/connectors-ootb/README.md) have been provided, or you can [create your own connectors](../modeling-connectors/connectors-create.md).
