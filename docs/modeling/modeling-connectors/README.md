---
Title: Connectors
---

# Connectors 
Connectors are used to execute logic outside of processes. Values are sent from a process to a connector to be used as part of the logic and the results sent back to the process afterwards.

Examples of what connectors can be used to action in a process include:

* automating emails using values from a process to populate the fields 
* interacting with an installation of Slack to post messages
* calling a custom REST API and passing the response back to the process 

Connectors conceptually contain two parts:

* An [image](#connector-images) of the connector that is deployed alongside an application. This image needs to contain the logic that the connector executes outside of the runtime bundle. 

* A [connector definition](#connector-definitions) that describes the actions a connector can make and the parameters that need to be passed to the connector image. The connector definition is used to attach a connector action to a service task within a process definition. 

Connector components can also be [extended further](https://www.alfresco.com/abn/adf/docs/process-services-cloud/) using the Application Development Framework. 

## Connector images
To execute logic outside of the runtime bundle (i.e. a process), a connector is deployed as a separate image but as part of the same deployment. The communication between the runtime bundle and connector is accomplished via specially named channels with messages managed by a message broker (Rabbit MQ by default). 

### Connector variables
The environment variables that are specific to each connector instance (or image) can be set as connector variables during the deployment of a released project. These variables are specific to the connector image and can include things such as a base URL for a REST endpoint or the host address of an external SMTP host. The connector image can then be deployed into the same namespace as the rest of an application's components. 

If different connector variables are required using the same connector image then multiple connector definitions will need to be defined that will use different instances of a connector image.

## Connector definitions
Connector definitions can be created or updated through the GUI or using the inbuilt JSON editor. A connector must have a name and description followed by an unlimited number of actions that each contain input and output parameters of data type; boolean, date, integer or string. 

A single connector can have multiple actions, for example creating a Slack channel and sending a message to a Slack channel are different actions but they can be handled by the same connector.  

Within each action of a connector definition input and output parameters need to be defined:

*  Input parameters are those that are sent from the process to the connector
*  Output parameters are those that are sent from the connector to the process 

Input and output parameters are paired to [process variables](../../modeling/modeling-processes/README.md#process-variables) within a process definition to pass values for each parameter back and forth and optionally reuse those values later on in a process. Alternatively, the parameters can be entered as values directly into a service task. 

Once a connector definition has been created, they are attached using the `implementation` value of a [service task](../../modeling/modeling-processes/processes-bpmn/bpmn-service.md) within a process definition using the format `<connector-name>.<action-name>`. Each service task can only execute a single connector action. The following is an example of the XML for a service task of a Slack connector executing the send message action: 

```xml
<bpmn2:serviceTask id="ServiceTask_3xcd7zp" implementation="slackConnector.SEND_MESSAGE" />
```

**Important**: Each service task that a connector is attached to can only execute a single action. However, the same connector can be used multiple times within the same, or different process definitions to execute the same action or different actions, as long as they are attached to different service tasks. 

If different [connector variables](#connector-variables) are required then multiple connector definitions will need to be defined that will use different instances of a connector image.

## Creating connectors
A set of [OOTB (out of the box) connectors](../modeling-connectors/connectors-ootb/README.md) have been provided, or you can [create your own connector](../modeling-connectors/connectors-create.md).
