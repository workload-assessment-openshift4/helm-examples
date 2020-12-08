# Application
This chart will deploy a simple Spring Boot application.

## Installation Guide

After checking out the repo, Run the following command in *this* directory: 

`helm install <name-of-app> .`

## Parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `configmap.greeting`                      | Greeting that will be displayed by the Spring application.                                                           | `This is the <environment> environment!`                     |


**Note**: If upgrading or otherwise changing the ConfigMap, the pods will need to be recreated to retrieve the updated value.

TODO: Documentation on relevant variables.

## Helm Chart Components

### Chart.yaml file

The `Chart.yaml` file is required for all Helm charts. It describes what the chart is, either an _application_ or _library_ chart. The `Chart.yaml` file also contains the _version_ and _appVersion_ of the application our example Helm chart is deploying. When making changes to a chart, it is important that the _version_ and _appVersion_ numbers are bumped up to reflect the new version of the application being deployed with the Helm Chart.

### Templates folder

The templates/ directory is one of the most integral parts of a Helm chart. The `templates` directory is where Helm finds the YAML definitions for your Kubernetes objects such as Services and Deployments.

#### deployment.yaml

Deployments determine the basic details required to deploy an application on OpenShift. One of these basic details consists of the container image that Openshift should deploy. 

Along with specifying the container image, Deployments also specify the number of replicas, or instances, of an application to deploy. Deployments can additionally define an application's resource limits, health checks, and volume mounts.

Our `deployment.yaml` file will describe the Deployment resource that will be created upon deployment of the Helm chart. 

#### service.yaml

A Service serves as an internal load balancer. Services allow users and other applications to talk to each other by allocating a static IP address to a Service endpoint. A Service then uses a label selector to find all the containers running that provide a certain network service on a certain port. 

Our `service.yaml` file describes the Service resource that will be created when deploying the Helm chart.

#### route.yaml
The `route.yaml` file is used to create a Route resource. A Route is a way to expose a Service by giving it an externally-reachable hostname such as: `www.example.com`. 


#### values.yaml file

The `values.yaml` file provides us access to values passed into the Helm templates. Within this file we can customize values our Openshift resources will utilize upon installation of the chart into the cluster. These values can also be overriden with the `--set` command upon installation of a chart.

This file is the default file when performing helm installations; if you have any additional variables or variables you want to overwrite you can create another values file and pass it as a parameter when running the helm command.

**Ex**: `helm install <name-of-app> . --set configmap.greeting="Hey there!" --values=values-dev.yaml`