# Application
This chart will deploy a simple Spring Boot application.

## Installation Guide

1. After checking out the repo, Run the following command in *this* directory: 

`helm install <name-of-app> . --set configmap.greeting=<message>`

**Note**: If upgrading or otherwise changing the ConfigMap, the pods will need to be recreated to retrieve the updated value.

TODO: Documentation on relevant variables.

