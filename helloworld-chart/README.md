# Application
This chart will deploy a simple Spring Boot application.

## Installation Guide

1. After checking out the repo, Run the following command in *this* directory: 

`helm install <name-of-app> . --set domain <domain>  --set configmap.greeting=<message>`

**Example**: 
`helm install test . --set domain apps.cluster-ccf2.sandbox801.opentlc.com  --set configmap.greeting="Hello!"`

**Note**: If upgrading or otherwise changing the ConfigMap, the pods will need to be recreated to retrieve the updated value.

TODO: Maybe add possible variables and what they mean/affect?

## Helm Chart Components

### Chart.yaml

The 'Chart.yaml' file is required for all Helm charts. It describes what the chart is, either an _application_ or _library_ chart. The 'Chart.yaml' file also contains the _version_ and _appVersion_ of the application our example Helm chart is deploying. When making changes to a chart, it is important that the _version_ and _appVersion_ numbers are bumped up to reflect the new version of the application being deployed with the Helm Chart.

### Templates

The templates/ directory is one of the most integral parts of a Helm chart. The _templates_ directory is where Helm finds the YAML definitions for your Kubernetes objects such as Services and Deployments.

#### deployment.yaml

Deployments determine the basic details required to deploy an applicatin on OpenShift. One of these basic details consists of the container image that Openshift should deploy. 

Along with specifying the container image, Deployments also specify the number of replicas, or instances, of an application to deploy. Deployments can additionally define an application's resource limits, health checks, and volume mounts.

Our 'deployment.yaml' file will describe the _Deployment_ resource that will be created upon deployment of the Helm chart. 

#### service.yaml

A Service serves as an internal load balancer. Services allow users and other applicatoins to talk to each other by allocating a static IP address to a Service endpoint. A Service then uses a label selector to find all the containers running that provide a certain network service on a certain port. 

Our 'service.yaml' file describes the _Service_ resource that will be created when deploying the Helm chart.  

### values.yaml

The 'values.yaml' file provides us access to values passed into the Helm chart. Within this file we can customize values our application will utilize upon deployment. An advantage of the 'values.yaml' file is that its values can be referenced within other configuration files within the chart. This allows us to parameterize certain aspects of our 