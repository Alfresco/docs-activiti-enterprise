---
Title: Database upgrades
---

# Database upgrades
Database upgrade scripts are used to upgrade the databases used in Activiti Enterprise between releases. 

To upgrade a database, select the correct tag for the version you are upgrading to and apply the script that relates to your database provider to your database.

**Note**: If upgrading through multiple versions, apply the upgrade scripts sequentially.

## Databases
All Activiti Enterprise databases use [Liquibase](http://www.liquibase.org/) to create upgrade scripts except for the [runtime bundle](../architecture/application.md#runtime-bundle).

The changelogs for each release are found in the `/config/<service-name>/liquibase/changelog` folder of each repository for H2 and Postgres databases: 

* [Modeling service](https://github.com/Alfresco/alfresco-modeling-service/tree/develop/src/main/resources/config/modeling/liquibase/changelog)
 
* [Deployment service](https://github.com/Alfresco/alfresco-deployment-servicetree/develop/src/main/resources/config/deployment/liquibase/changelog) 

* [Audit service](https://github.com/Activiti/activiti-cloud-audit-service/blob/develop/activiti-cloud-starter-audit/src/main/resources/config/audit/liquibase/changelog/)
 
* [Form service](https://github.com/Alfresco/alfresco-form-service/tree/develop/src/main/resources/config/form/liquibase/changelog)

* [Query service](https://github.com/Activiti/activiti-cloud-query-service/blob/develop/activiti-cloud-starter-query/src/main/resources/config/query/liquibase/changelog/)

## Runtime bundle
The runtime bundle upgrade scripts are created using a custom approach. 

The [upgrade scripts](https://github.com/Activiti/Activiti/blob/7.1.0.M6/activiti-core/activiti-engine/src/main/resources/org/activiti/db/upgrade/) are named slightly differently for the runtime bundle, for example:

```
activiti.postgres.upgradestep.7.1.0-M5.to.7.1.0-M6.engine.sql
```