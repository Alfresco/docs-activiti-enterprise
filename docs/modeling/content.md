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

* Note about prefixing the namespace name to custom things

## Properties

Content model:

| Property | Description | Example | Required? | 
| -------- | ----------- | ------- | --------- | 
| `Namespace` | A namespace unique within the repository for the content model to sit under. This ensures that all custom types, aspects and properties are also unique within the repository. Namespaces can only contain alphanumeric characters and must be URIs. | http://finance.com/model/accounts/1.0 | Yes | 
| `Prefix` | An abbreviation of the `namespace` of a content model to refer to types and aspects without needing to use the full namespace. Prefixes can only contain alphanumeric characters, hyphens and underscores. | fin-acc | Yes | 
| `Name` | The name of the content model. Content model names can only contain alphanumeric characters, hyphens and underscores. | Financial Accounts Model | Yes | 
| `Creator` | The author of the model. The default value is the currently signed in user. | hruser | No | 
| `Description` | A free text description of what the content model is for | A content model for recording financial accounts records. | No | 





Custom type: 

| Property | Description | Example | Required? | 
| -------- | ----------- | ------- | --------- | 
| `Name` | The name of the custom type. Custom type names can only contain alphanumeric characters, hyphens and underscores. |  | Yes |
| `Parent type` | A parent type for the custom type. All properties and aspects assigned to the parent are inherited by the child. | cm:content | Yes | 
| `Title` | A display label for the custom type that will be displayed to users | | No | 
| `Description` | A free text description of what the custom types is for | | No | 




Custom aspect:

| Property | Description | Example | Required? | 
| -------- | ----------- | ------- | --------- | 
| `Name` | The name of the custom aspect. Custom aspect names can only contain alphanumeric characters, hyphens and underscores. |  | Yes |
| `Parent aspect` | A parent aspect for the custom aspect. All properties and aspects assigned to the parent are inherited by the child. | smf:smartFolder | No | 
| `Title` | A display label for the custom aspect that will be displayed to users | | No | 
| `Description` | A free text description of what the custom aspect is for | | No | 


Custom property:

| Property | Description | Example | Required? | 
| -------- | ----------- | ------- | --------- | 
| `Name` | The name of the property. Property names can only contain alphanumeric characters, hyphens and underscores. |  | Yes |
| `Title` | A display label for the property that will be displayed to users | | No | 
| `Description` | A free text description of what the property is for | | No | 
| `Data type` | The data type of the property | d:float | No | 
| `Mandatory` | Set whether the property is mandatory | true | No | 
| `Multiple` | Set whether the property can contain multiple values | false | No | 
| `Default value` | Set a default value for the property | | No | 


Data types table? 


## Permissions

* Requires the user to be in the group ALFRESCO_MODEL_ADMINISTRATORS group in ACS (so need to note that permissions are managed different to the other models. 





 
