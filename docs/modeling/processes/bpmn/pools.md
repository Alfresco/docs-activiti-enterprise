---
Title: Pools and lanes
---

# Pools and lanes 
Pools are used to model different participants in a process definition such as different organizations or different business personas such as a customer and a manufacturer. Lanes are used to graphically represent different roles within an organization such as by different departments.

## Pools
Pools allow multiple processes to be modeled in a single process definition. The scope of [process variables](../../processes/variables.md) are restricted to each process itself. Only [message events](../bpmn/message.md) can be used to communicate between the two processes. 

An example of using two pools would be for a customer to fill out an order in one process that sends a message on order completion triggering a second process for a warehouse team to action. The two processes would have different process instance IDs at runtime but would both be using the same process definition. 

### Properties

The properties for pools are: 

| Property | Description | Example | Required | 
| -------- | ----------- | ------- | -------- | 

## Lanes
Lanes are used to display the different personas affecting a process to the modeler. Lanes have no impact on a process at runtime. Lanes can also be nested for example to show different teams within a department. 







