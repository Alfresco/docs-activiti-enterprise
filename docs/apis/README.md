---
Title: REST APIs
---

# REST APIs
The REST APIs for Activiti Enterprise are accessed differently depending on whether a service is application or platform specific. If a service is application specific then the *application name* will form part of the endpoint. 

## Platform endpoints
The OpenAPI specifications for platform endpoints are as follows:

* Modeling service: `gateway.{domain-name}/modeling-service/swagger-ui.html`
* Deployment service: `gateway.{domain-name}/deployment-service/swagger-ui.html`

## Application endpoints
The OpenAPI specifications for application endpoints require the `{application-name}` element in the URL as demonstrated in the following:

* Runtime bundle: `gateway.{domain-name}/{application-name}/rb/swagger-ui.html`
* Form service: `gateway.{domain-name}/{application-name}/form/swagger-ui.html`
* Query service: `gateway.{domain-name}/{application-name}/query/swagger-ui.html`
* Audit service: `gateway.{domain-name}/{application-name}/audit/swagger-ui.html`

### Notification service
The notification services uses [GraphQL](https://graphql.org/learn/) to expand on querying. It reads from the same data store as the query service and can be accessed at the following URL: 
`gateway.{domain-name}/{application-name}/graphiql`. 