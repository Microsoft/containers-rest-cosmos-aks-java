# Containerized Java REST Services on Azure Kubernetes Service with a CosmosDB backend

<!-- ## Project Health

API Build Status: [![Build Status](https://dev.azure.com/csebostoncrew/ProjectJackson/_apis/build/status/GitHub%20Builds/ProjectJackson-API-GitHub?branchName=master)](https://dev.azure.com/csebostoncrew/ProjectJackson/_build/latest?definitionId=22?branchName=master)

UI Build Status: [![Build Status](https://dev.azure.com/csebostoncrew/ProjectJackson/_apis/build/status/GitHub%20Builds/ProjectJackson-UI-GitHub?branchName=master)](https://dev.azure.com/csebostoncrew/ProjectJackson/_build/latest?definitionId=25?branchName=master)

Infrastructure Build Status: [![Build Status](https://dev.azure.com/csebostoncrew/ProjectJackson/_apis/build/status/GitHub%20Builds/ProjectJackson-Infrastructure-GitHub?branchName=master)](https://dev.azure.com/csebostoncrew/ProjectJackson/_build/latest?definitionId=23?branchName=master)
-->
## Contents:

* Concept introduction & overview (this document)
* [Current & planned features](#current-and-planned-features) 
* [Quick Start for Developers](./GettingStarted.md)
* [Sample Application and REST APIs](./SampleApp.md)

## Introduction

This project has been created to emulate several best practices that can be adopted while building and deploying applications on Azure.
This document explains what the project provides and why, and it provides instructions for getting started. This project is work in progress as we continually evolve the deployment code. 

The project has been initially forked from a [repo](https://github.com/Microsoft/containers-rest-cosmos-appservice-java) that deploys the same application on Azure App Services. 

## Enterprise-Class Applications Defined

We are using the term "enterprise-class app" to refer to an end-to-end solution that delivers the following 
capabilities:

* **Horizontal scalability:** Add capacity by adding additional containers and/or VMs
* **Infrastructure as code:** Create and manage Azure environments using template code that is under source control
* **Agile engineering and rapid updates:** Use CI/CD for automated builds, tests and deployments, safe code check-ins, and frequent updates to the production environment and application.
* **High Availability:** Design and deploy robust applications and infrastructure, so that the application continues to run normally even when some components fail or go offline.
* **Blue/Green (aka Canary Deployments):** Rollout updates to a "green" application instance, while the existing deployment continues to run on the "blue" instance. The green instance is intially exposed to only a small number of users. Monitoring is performed to look for any degradations in service related to the green instance. If everything looks good, traffic is gradually diverted to the green instance. Should the service quality degrade, the deployment is rolled back by returning all traffic to the blue instance.
* **Testable:** Continuously test the application in production to validate scalability, resilience, and security.
* **Hardened:** Assure that the application and infrastructure is instrinsically resistant to attacks from bad actors, such as Distributed Denial of Service (DDoS) attacks.
* **Networking compliance:** Comply with enterprise network security requirements, such as the use of ExpressRoute to communicate with enterprise data-centers and/or on-premises networks, and private IPs for all but public endpoints.
* **Monitoring and Analytics:** Capture telemetry to enable operations dashboards and automatic alerting of critical issues.
* **Service Authentication:** Allow only authorized access to services via token- or certificate-based service authentication.
* **Simulated Traffic:**
* **Chaos Testing:**

## OSS Technology Choices

Our team, Commercial Software Engineering (CSE), collaboratively codes with Microsoft's biggest and most important customers.
We see a huge spectrum of technology choices at different customers, ranging from all-Microsoft to all-OSS. More commonly, we see a mix.

Given the wide range of technology choices, it's difficult to create a one-size-fits-all solution. For this project, we selected a set of technologies that are of interest to many of our customers.


This OSS solution uses the following OSS technologies:

* **GitHub:** Publishing this project to GitHub indicates our desire to share it widely and to encourage community contributions. 
* **Docker:** Though there are other container technologies out there, Docker/Moby is pretty much synonymous with the idea.
* **Java Version 8 (1.8.x):** A very common choice of programming langauages by many enterpises.
* **Spring Boot:** One of the most widely used and capable Java frameworks.
* **Spring Data REST:** A simple way to build REST APIs in a Spring Boot application that are backed by a persistent data repository.
* **Maven:** A commonly used tool for building and managing Java projects.
* **Kubernetes:** An open-source system for automating deployment, scaling, and management of containerized applications (Additional OSS tools used in the logging, monitoring and reverse proxy.)
* **Gitops:** A way to do Continuous Delivery

## Azure Technologies & Services

As with our OSS technology choices, we intentionally selected a set of Azure technologies and services that support common enterprise requirements, including:

* **Azure DevOps:** Microsoft's CI/CD solution, which is the Azure-branded version of Microsoft's mature and widely used VSTS solution.
* **Azure Resource Manager (ARM):** Azure's solution for deploying and managing Azure resources via JSON-based templates.

* **Cosmos DB:** Cosmos DB is Microsoft's globally distributed, horizontally scalable, and highly available NoSQL data storage service. It is an excellent choice when performance and reliability are a must, and when enterprises require multi-region write capabilities, which are essential for both application/service performance and for HA/DR scenarios.
* **Azure Traffic Manager:** DNS-based routing service to connect users to the nearest data center. Redirects traffic to healthy location when another region goes offline. Also enables recommended method blue-green (aka canary) deployments with Azure App Services.
* **Azure Monitoring:** It allows to collect granular performance and utilisation data, activity and diagnostics logs, and notifications from your Azure resources in a consistent manner.
* **Key vault:** Enterprise developers use App Insights to monitor and detect performance anomalies in production applications.

The solution leverages Azure Dev Ops for Continuous Integration 
and Delivery (CI/CD), and it deploys complete Azure environments via Azure Resource Manager (ARM) templates.

## Key Benefits

Key technologies and concepts demonstrated:

| Benefit | Supporting Solution
|---|---
| Common, standard technologies | <li>Java programming language<li>Spring Boot Framework, one of the most widely used frameworks for Java<li>MongoDB NoSQL API (via Azure Cosmos DB)<li>Redis Cache
| Containerization | Microservices implemented in Docker containers, hosted by the Azure Kubernetes service.
| CI/CD pipeline | Continuous integration/continuous delivery (CI/CD) is implemented using Azure DevOps and Gitops with a pipeline of environments that support dev, testing and production
| Automated deployment | <li>Azure ARM templates<li>Azure Kubernetes Service<li>Azure container registry
| High Availability/Disaster Recovery (HA/DR) | Full geo-replication of containers and data, with automatic failover in the event of an issue in any region:<br><br><li>Cosmos DB deployed to multiple regions with active-active read/write<li>Session consistency to assure that user experience is consistent across failover<li>Stateless microservices deployed to multiple regions<li>Health monitoring to detect errors that require failover<li>AKS allows you scaling resources not only vertically but also horizontally, easily and quickly
| Demonstrates insfrastructure best practices | <li>Application auto-scaling<li>Minimize network latency through geo-based DNS routing<li>API authentication<li>Distributed denial of service (DDoS) protection & mitigation
| Proves application resiliency through chaos testing | A Chaos Monkey-style solution to shut down different portions of the architecture in order to validate that resilience measures keep everything running in the event of any single failure

## Current and Planned Features

We are continually evolving the code incorporating best practices, and will be documenting the changes as we make. This section will always be updated and you can use this as a reference to identify what the solution entails and what's in progress.


## Current

| Component | Description
|---|---
| Spring Boot Application based on IMDB Data | This is a sample application and you can easily replace with your own application
| IMDB Data import scripts | The scripts to load and store IMDB data in Azure Cosmos DB using the Mongo API, you can replace this with your own cosmos DB data
| Build pipeline | Built in Azure DevOps, the pipeline creates an image with the application and pushes the image into Azure Container Registry
| Deployment pipeline | Built using GitOps, triggers deployment using tags from Azure Container Registry into an Azure Kubernetes Cluster
| Azure Kubernetes Cluster | Kubernetes cluster deployed through Helm Charts on Azure Kubernetes Service (AKS), with basic load balancer service, auto-scaling of pods 
| Azure Keyvault Integration | The deployment pipeline uses the Azure Key Vault service to store & refer to secrets at Pod level. The code is a current implementation of a [work-around](https://github.com/Azure/kubernetes-keyvault-flexvol/issues/28) till native Key Vault service support is enabled.
| Azure Traffic Manager | Traffic manager service in Azure to re-direct traffic across multiple geo-clusters. Currently this will be pointing to the single cluster deployed.

## Planned

| Component | Description
|---|---
| Integration with [Istio](https://istio.io/docs/concepts/what-is-istio/)  | Include the integration with Istio for ingress traffic management, inter-service communication , enabling canary deployments  
| Monitoring & Logging | Integrate with [Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/overview) to log critical services and application data, build dashboards.
| Geo-replication | Deploying the application into three clusters in three different regions in active-active mode, with traffic manager routing traffic based on geo-affinity rules
| Performance & Chaos Testing Pipelines | Creating a performance testing deployment pipeline, and a chaos testing pipeline that can each automate  testing
| Blue/Green Canary Deployments | Enabling canary deployments for selected blue/green traffic on different versions of application, using Istio.



## Contribute

See [CONTRIBUTING.md](./CONTRIBUTING.md) for more information.
