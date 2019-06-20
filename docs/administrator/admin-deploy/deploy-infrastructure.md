---
Title: Customizing application infrastructure
---

# Customizing application infrastructure


* [Configuring external Postgres instances](#configuring-external-postgres-instances)
* [Deploying an application with custom images](#deploying-an-application-with-custom-images)

## Configuring external Postgres instances
By default, data for the audit service, query service, notification service, preference service, form service and runtime bundle is stored in an embedded instance of Postgres deployed into an application namespace using separate schemas. A number of options are available to update the Postgres storage options: 

* [Use an external Postgres instance for all services](#use-an-external-postgres-instance-for-all-services)
* [Use separate an external Postgres instance(s) for each service](#use-an-external-postgres-instance(s)-for-each-service)

### Use an external Postgres instance for all services
If the variables for the `postgresql-service` are updated to point to an external instance of Postgres, all services will use this external instance. 

The following is an example deployment payload specifying these variables:

```json
{
  "name": "external-postgres-application",
  "releaseId": "29925e2b-ef26-49df-9156-ea3a2a910c42",
  "security": [
    {
      "role": "APS_ADMIN",
      "users": ["..."]
    },
    {
      "role": "APS_USER",
      "users": ["..."]
    }
  ],
  "variables": {
	    "postgresql-service":{
        "url":"jdbc:postgresql://aaedb.cpcs2n7mznht.us-east-1.rds.amazonaws.com:5432/aaedb",
        "username": "aae",
        "password": "password",
        "jpaDatabasePlatform":"org.hibernate.dialect.PostgreSQLDialect",
        "ddlAuto": "update"
      }
  }
}
```

In the Administrator Application, this would look similar to the following: 

![Example external Postgres deployment variables](../../images/deploy-postgres-external.png)

### Use an external Postgres instance(s) for each service
An external instance of Postgres can be specified at the specific service level. This means that as few or as many services can use an external instance as required. The external instances can be different for each service. Any service that does not have a Postgres instance specified will use the `postgresql-service` settings as its default. 

The following is an example deployment payload where the query service is set to use an external instance of Postgres: 

```json
{
  "name": "application-external-postgres",
  "releaseId": "29925e2b-ef26-49df-9156-ea3a2a910c42",
  "security": [
    {
      "role": "APS_ADMIN",
      "users": ["..."]
    },
    {
      "role": "APS_USER",
      "users": ["..."]
    }
  ],
  "variables": {
	  "query-service": {
		  "SPRING_DATASOURCE_URL":"jdbc:postgresql://aaedb.cpcs2n7mznht.us-east-1.rds.amazonaws.com:5432/apsdb",
		  "SPRING_DATASOURCE_USERNAME": "aae",
		  "SPRING_DATASOURCE_PASSWORD": "password",
		  "SPRING_JPA_DATABASE_PLATFORM":"org.hibernate.dialect.PostgreSQLDialect",
		  "SPRING_JPA_HIBERNATE_DDL_AUTO": "update"
	  }
  }
}
```

In the Administrator Application, this would look similar to the following:

![Example external Postgres deployment variables for the query service](../../images/deploy-postgres-query.png)

## Deploying an application with custom images
It is possible to replace the Activiti Enterprise images for certain services with custom images based on the [Activiti 7 starters](http://www.activiti.org/). The services that can be replaced are the following:

* [Audit service](https://github.com/Activiti/activiti-cloud-audit-service)
* [Query service](https://github.com/Activiti/activiti-cloud-query-service)
* [Runtime Bundle](https://github.com/Activiti/activiti-cloud-runtime-bundle-service)

### Deployment steps
Custom images need to be pushed to the Kubernetes registry before deploying an application that uses them:

```
docker push registry.$DNSNAME/<custom-docker-image-name>:master
```

Once a custom image is in the Kubernetes registry of your deployment, the location can be set for the service it is replacing and any custom variables configured during [application deployment](../admin-deploy/README.md#deployment-steps).

The following is an example of the payload for setting a custom image for the query service during deployment: 

```json
"infrastructure"."query-service"."image"="registry.aae.alfresco.com/alfresco/custom-query-service:latest"
```

### Other services
Other services such as the DMN, Form, Preference and Process Storage services can have their images replaced with custom ones. These services do not have Activiti 7 starters available to them and will need to be created manually. 
