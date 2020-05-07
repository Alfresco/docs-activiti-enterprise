---
Title: Scripts
---

# Scripts 
Scripts are used to execute a script as part of a process. Process variables can be passed to the script and the results of a script can be sent back to a process instance as process variables.

Scripts are executed as part of the [application runtime bundle](../architecture/application.md#application-runtime-bundle) with the results being returned to the process engine after execution. Script design uses the functionality of  [Monaco](https://github.com/Microsoft/monaco-editor) and the [Graal javascript engine](https://github.com/graalvm/graaljs) for execution. 

Scripts can be added to a process definition by using a [script task](../modeling/processes/bpmn/script.md).

The properties of a script are:

| Property | Description | Example | Required |
| -------- | ----------- | ------- | -------- |
| `Name` | The name of the script. Script names must be in lowercase and between 1 and 26 characters in length. Alphanumeric characters and hyphens are allowed, however the name must begin with a letter and end alphanumerically. | order-script | Yes |
| `Language` | The development language the script is written in | Javascript | Yes | 

## Modeling scripts

### Variables
There are two types of variables associated with a script. Script variables are stored in the `<script-name>-extensions.json` file and declared variables are declared as part of the script itself.

#### Script variables
Script variables are used to pass values between a script and a process using script variables and [process variables](../modeling/processes/variables.md) with the mapping between the two configured in the [script task](../modeling/processes/bpmn/script.md) itself. Script variables can be configured using the **Edit Script Variables** button in the properties panel of the GUI or the JSON editor. 

The following are the data types that script variables can be set as:

| Type | Description | Example | 
| ---- | ----------- | ------- |
| String | A sequence of characters | `#Mint-Ice-Cream-4!`
| Integer | A positive whole number | `642` |
| Boolean | A value of either `true` or `false` | `true` |
| Date | A specific date in the format `YYYY-MM-DD` | `2020-04-22` | 
| Datetime | A specific date and time in the format `YYYY-MM-DD HH:mm:ss` | `2020-09-10 22:30:00`
| File | A [file](../modeling/files.md) uploaded into a process definition or as part of a process instance or task | 
| JSON | A JSON object | `{"flavor" : "caramel"}` | 

Script variables are stored in the `variables` of the `<script-name>-extensions.json` and can also be viewed through the UI in the **Metadata**.

The following is an example of a `<script-name>-extensions.json` file or metadata with three script variables:

```json
"variables": [
	{
	"id": "4d32427e-23c2-4e49-8ecf-77ce0bd4e575",
	"name": "cost",
	"type": "integer",
	"value": 10
	},
	{
	"id": "cc31fe8b-db7c-4055-94d8-7ad5f15ef263",
	"name": "orders",
	"type": "integer"
	},
	{
	"id": "004826f3-d2f9-4fff-a905-17e2c4dd38e0",
	"name": "totalCost",
	"type": "integer"
	}
]
```

#### Declared variables
Declared variables are used within the script itself and can be set to the value of a script variable by using the prefix `variables.` to reference the script variable. An input variable will set a declared variable to the value of a script variable when the script is executed. 

For example, in a process the script variables `cost` and `orders` will have their values set  from process variables. The declared variables `costOfItem` and `numberOfOrders` can then be set to these values using the following:

```javascript
let costOfItem = variables.cost;
let numberOfOrders = variables.orders;
```

The value of the script variable `totalCost` will then be set after the script has executed by using the following: 

```javascript
variables.totalCost = costOfItem * numberOfOrders;
```

The value of the script variable `totalCost` can finally be sent back to the process by [mapping it to a process variable](../modeling/processes/variables.md).

### Process runtime
Payloads can be created for the [runtime bundle](../architecture/application.md#runtime-bundle) and then sent to a channel. For example to create a process instance using a [process definition ID](../modeling/processes/README.md) and setting starting variables:

```javascript
let startProcessInstanceCmd = processPayloadBuilder.start()
	.withProcessDefinitionKey("model-1bs32339-2wc2-4af2-9496-e9a031f12145")
	.withVariable("orderNumber": variables.orderNumber)
	.withVariable("quantity": variables.quantity)
	.build();
commandProducer.send(startProcessInstanceCmd);
```

### Content APIs
Four content APIs can be used within a script to start actions against the [content repository](../architecture/platform.md#alfresco-content-services). 

#### nodesAPI
The [nodes API](https://api-explorer.alfresco.com/api-explorer/#/nodes) is used to perform operations on nodes within Alfresco Content Services (ACS) such as creating new nodes, moving existing nodes to a different parent and updating node content. 

Use the following syntax to create the `@body` for the API calls:

```javascript
let nodeBodyCreate = nodeBody.create();
let nodeBodyUpdate = nodeBody.update();
let nodeBodyLock = nodeBody.lock();
let nodeBodyMove = nodeBody.move(); 
```

#### peopleAPI
The [people API](https://api-explorer.alfresco.com/api-explorer/#/people) is used to perform operations on individual users within Alfresco Content Services (ACS) such as creating new users, updating existing ones and resetting user passwords.

Use the following syntax to create the `@body` for the API calls:

```javascript
let personBodyCreate = personBody.create();
let personBodyUpdate = personBody.update();
let passwordResetBody = passwordBody.reset();
```

#### groupsAPI
The [groups API](https://api-explorer.alfresco.com/api-explorer/#/groups) is used to perform operations on user groups within Alfresco Content Services (ACS) such as creating a new group and updating the membership of a group.

Use the following syntax to create the `@body` for the API calls:

```javascript
let groupBodyCreate = groupBody.create();
let groupBodyUpdate = groupBody.update();
let groupMembershipBodyCreate = groupMembershipBody.create();
```

#### siteAPI
The [sites API](https://api-explorer.alfresco.com/api-explorer/#/sites) is used to perform operations on sites in Alfresco Content Services (ACS) such as creating new sites, updating the membership of a site and managing site membership requests. 

Use the following syntax to create the `@body` for the API calls:

```javascript
let siteBodyCreate = siteBody.create();
let siteBodyUpdate = siteBody.update();
let siteMembershipBodyCreate = siteMembershipBody.create();
let siteMembershipBodyUpdate = siteMembershipBody.update();
let siteMembershipBodyApproval = siteMembershipBody.approval();
let siteMembershipBodyRequestCreate = siteMembershipBody.requestCreate();
let siteMembershipBodyRejection = siteMembershipBody.rejection();
let siteMembershipBodyRequestUpdate = siteMembershipBody.requestUpdate();
```

## Simulation
Once a script has been modeled it can be tested using the [script service](../architecture/platform.md#script-service).

In the UI click the **Simulate** button after entering the values for each script variable. The results will be populated in the outputs section. 

