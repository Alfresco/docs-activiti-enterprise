---
Title: Technology
---

# Technology
Whilst a brief overview of cloud native applications and short introduction to the technologies and concepts used in and with Alfresco Activiti Enterprise is provided, it is recommended that you have at least a basic understanding of the following before getting started:  

* Microservice architectures
* Containerization; Docker, Helm and Kubernetes
* Spring Boot and Spring Cloud technologies
* Continuous Integration and Continuous Delivery (CI/CD) 

## Overview
Cloud native applications differ from traditional monolithic applications as they split out the architecture of an application into a number of smaller functions or processes referred to as microservices. These microservices can be considered self-contained, with communication only possible via APIs (Application Programming Interfaces). The advantage to microservices is that they can be developed, upgraded or replaced independently of one another. 

Microservices are normally run using containers as this separates them from the host operating system and allows them to be consumed cross-platform. In addition to this portability, containers are very lightweight in comparison to something such as a virtual machine (VM) which means that they can be rapidly scaled and deployed. An orchestration software is used to monitor these deployed containers that will dynamically scale based on the load hitting the application and automatically restart any containers that have gone down. 

## Spring
Alfresco Activiti Enterprise was built using [Spring](https://spring.io/) technology.  

[Spring Boot](https://spring.io/projects/spring-boot) is considered a Java framework and is used to help generate microservice projects and applications. [Spring Cloud](https://spring.io/projects/spring-cloud) provides tools for developers that are building distributed applications that attempt to simplify aspects of the build process. 

## Containers (Docker) 
[Docker](https://www.docker.com/) is an open source software that deals with containers. Containers can be thought of as a way of packaging software applications. All of the code, dependencies and libraries of an application are put into a container as an immutable artifact. The most important concept of containers is that they are predictable because of this immutability - they will always behave in the same manner regardless of where you run them.  

A conceptual comparison can be drawn between containers and virtual machines. The key difference is that containers don’t include an operating system which drastically decreases their start up time. This is an important distinction and advantage when using containers in a microservice architecture.

Docker uses a file called a dockerfile that defines the commands used in the process of building an immutable Docker Image. A Docker Image is essentially a snapshot of your application that is ready to be started as a container in any environment.

Further information is available in the [Docker documentation](https://docs.docker.com/). 

## Container Orchestration (Kubernetes) 
[Kubernetes](https://kubernetes.io/) is an open source system used to automate aspects of deployment, scaling and management in containerized applications. Kubernetes reduces the amount of manual intervention required for maintaining applications through this automation. 

For scaling, Kubernetes automatically increases or reduces the number of containers based on the demand on services. It also manages the distribution of load across the containers it starts up and restarts failed pods (a pod is a group of one or more containers). 

Kubernetes advocates for constant uptime that a user expects. Rather than having to restart the entire infrastructure, a new container can be created and released with a new tag and once that is up and running, the old image container is stopped. This approach heavily benefits a microservice architecture, as it allows the individual services to be updated, tested and released independently of each other. 

Further information is available in the [Kubernetes documentation](https://kubernetes.io/docs/home).  

## Container Packaging (Helm)
[Helm](https://helm.sh/) is an open source package manager for Kubernetes and calls these packages Helm Charts. Helm has a client that you interact with on the command line and a component called Tiller that manages Helm Chart releases and history that is installed in your Kubernetes cluster. 

A Helm Chart describes a set of Kubernetes resources to deploy. A Helm chart can be dependent upon any number of other charts in order to describe complicated deployments - in a similar way to how microservices split up an application into its component parts. 

Importantly, Helm offers a way of managing versions of templates for application deployments into Kubernetes.

These templates, or Helm Charts are stored in repositories that can be public or private so they can be reused. They can also be configured to allow variables to be set by users that are specific to their own needs.

Further information is available in the [Helm documentation](https://docs.helm.sh/). 

## Continuous Integration and Continuous Delivery CI/CD 
Continuous integration and continuous delivery (CI/CD) are the practice of trying to reduce the time taken for developmental changes to reach a production environment. Changes should ideally have the same tests run against them as before they were made and their deployment should be automated. 

An example of a CI/CD tool is [Jenkins X](https://jenkins-x.io/). It is aimed specifically at Kubernetes environments.