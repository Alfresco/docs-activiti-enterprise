---
Title: Database upgrades
---

# Database upgrades
Database upgrade scripts are used to upgrade the databases used in Activiti Enterprise between releases. 

To upgrade a database, select the correct tag for the version you are upgrading to and apply the script that relates to your database provider to your database.

**Note**: If upgrading through multiple versions, apply the upgrade scripts sequentially.

## Databases
All Activiti Enterprise databases use [Liquibase](http://www.liquibase.org/) to create upgrade scripts except for the [application runtime bundle](../architecture/application.md#application-runtime-bundle).

The changelogs for each release are found in the `/config/<service-name>/liquibase/changelog` folder of each repository for H2 and Postgres databases.

## Runtime bundle
The runtime bundle upgrade scripts are created using a custom approach. 

The [upgrade scripts](https://github.com/Activiti/Activiti/blob/7.1.0.M7/activiti-core/activiti-engine/src/main/resources/org/activiti/db/upgrade/) are named slightly differently for the runtime bundle, for example:

```
activiti.postgres.upgradestep.7.1.0-M5.to.7.1.0-M6.engine.sql
```