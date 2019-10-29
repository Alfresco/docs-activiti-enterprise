---
Title: Creating connectors
---

# Creating a connector
There are four broad steps involved with creating your own connector:

1. Create a connector image that contains your logic that will run against process values.
2. Create a connector definition in the Modeling Application.
3. Configure a connector to a service task in a process definition.
4. Deploy a released project and connector image.

## Creating a connector image
More information and resources will be available shortly on how to create a connector image.

## Creating a connector definition 
A connector definition is required to link your connector to a [service task](../processes/bpmn/service.md) task within a process. To create a connector definition, sign into the Alfresco Modeling Application with a user that has permission to create projects. The user will require the *APS_MODELER* role.

**In the Connectors section of the Modeling Application**: 

1. Create a new connector definition and give it a name and optional description.
2. Create a new action and give it a name and optional description. 

	**Note**: The combination of connector name and action name will appear as the 	`implementation` value of a service task in the process definition XML. 

3. Create the input and output parameter(s) for your connector. Input parameters are those sent from the process to a connector and output parameters are those received back. 
4. Repeat Steps 2 and 3 for as many actions as your connector can execute.
5. Save your connector definition. It will now be available to attach to a service task in a process definition. 

## Configuring a connector within a process
To use your connector within a process, sign into the Alfresco Modeling Application with a user that has permission to create projects. The user will require the *APS_MODELER* role. 

**In the Processes section of the Modeling Application**:

1. Create a service task and use the `implementation` value dropdown to select your connector definition followed by the action that the service task will use.

	**Note**: if you view the XML editor you will see that the value of the `implementation` 	property is `<connector-name>.<action-name>` for the associated service task.

2. Assign the input and output parameters for the connector to [process variables](../processes/README.md#process-variables) within the process definition. If the names are identical this matching is done automatically. 

	**Note**: the mappings between process variables and connector actions can be viewed in 	the [`<application-name>-extensions.json` file](../projects.md#files) located in the processes folder of your 	project.

3. Complete and save your process. 

## Deploying a connector
A connector needs to be deployed with its associated released project using the [Alfresco Administrator Application to deploy](../../administrator/deploy/README.md). The location of the connector image and any connector variables are set during deployment.