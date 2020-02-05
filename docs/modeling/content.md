---
Title: Content models
---

# Content models
Content models 

* what is a content model
* What is a type
* What is an aspect
* What is a property (child of type or aspect) 

* status active or inactive

* permissions required to model with it 

## Properties

Content model:

| Property | Description | Example | Required? | 
| -------- | ----------- | ------- | --------- | 
| `Namespace` | A namespace unique within the repository for the content model to sit under. This ensures that all custom types, aspects and properties are also unique within the repository. Namespaces can only contain alphanumeric characters and must be URIs. | http://finance.com/model/accounts/1.0 | Yes | 
| `Prefix` | An abbreviation of the `namespace` of a content model to refer to types and aspects without needing to use the full namespace. Prefixes can only contain alphanumeric characters, hyphens and underscores. | fin-acc | Yes | 
| `Name` | The name of the content model. Names can only contain alphanumeric characters, hyphens and underscores. | Financial Accounts Model | Yes | 
| `Creator` | The author of the model. The default value is the currently signed in user. | hruser | No | 
| `Description` | A free text description of what the content model is for | A content model for recording financial accounts records. | No | 





Custom type:

| Property | Description | Example | Required? | 
| -------- | ----------- | ------- | --------- | 
| `Name` | The name of the custom type. Custom types can only contain alphanumeric characters, hyphens and underscores. |  | Yes |
| `Parent type` | 
| `Title` | 
| `Description` | 


Enter a name for the type and / or aspect.
Only alphanumeric characters, hyphens (-), and underscores (_) are allowed. The model name will automatically prefix the model namespace.

Select the parent type for the type and / or aspect.
Note:
The parent type must either be a sub-type of the cm:content type or cm:folder type. The custom type will inherit all the properties and aspects assigned to the parent.
Note:
The aspect will inherit all the properties and aspects assigned to the parent. The default parent type is none.
Enter an optional display label for the type and / or aspect.
The display label is shown to the users as the model name in the New Type drop down in Alfresco Share.

For example, Invoice.

Enter an optional description of the type and / or aspect.











Custom aspect:

| Property | Description | Example | Required? | 
| -------- | ----------- | ------- | --------- | 
| `Name` | 
| `Title` | 
| `Description` | 


Custom property:

| Property | Description | Example | Required? | 
| -------- | ----------- | ------- | --------- | 
| `Name` | 
| `Title` | 
| `Description` | 
| `Data type` | 
| `Mandatory` | 
| `Multiple` | 
| `Default value` | 





## Permissions

* Requires the user to be in the group ALFRESCO_MODEL_ADMINISTRATORS group in ACS (so need to note that permissions are managed different to the other models. 





 
