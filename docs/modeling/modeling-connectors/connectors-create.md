---
Title: Creating connectors
---

# Connector setup
There are two broad steps involved with creating your own connector and configuring it within a process:

1. Create and design a connector image that contains your business logic, published to an available platform such as Docker Hub. 
2. Import and/or create and configure your connector within the Alfresco Modeling Application and link it to service tasks within a process definition. 

Each of these steps can be broken down into smaller tasks. 

## Create a connector


## Configure a connector within a project
To use your connector within a project, sign into the Alfresco Modeling Application with a user that has permission to create projects. The user will require the APS_MODELER role. 

**In the connector section of the Modeling Application**: 

1. Name your connector and add a description.
2. Name your actions and add the appropriate input(s) and output(s).
3. Save the connector.

**In the process section of the Modeling Application**:

1. Create a service task and use the `implementation` value dropdown to select your connector name followed by the action that the service task will use.

	**Note**: if you view the XML editor you will see that the value of the `implementation` 	property is `connectorName.actionName` for the associated service task.

2. Assign the input(s) and output(s) for the connectors to process variables within the process. If the names are identical this matching is done automatically. 

	**Note**: the mappings between process variables and connector actions can be viewed in 	the `<applicationName>-extensions.json` file located in the processes folder of your 	project.

3. Complete and save your process. 

## Deploying a connector
A connector needs to be deployed with its associated project. If you are using the [Alfresco Administrator Application to deploy](../../administrator/admin-deploy.md) then your connector will need to be in an accessible repository such as Docker Hub so that it can be defined during the deployment steps. 