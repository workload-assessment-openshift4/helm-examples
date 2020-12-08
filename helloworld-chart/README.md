# Application
This chart will deploy a simple Spring Boot application.

## Installation Guide

After checking out the repo, Run the following command in *this* directory: 

`helm install <name-of-app> . --values=value-<environment>.yaml`

# Parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `configmap.greeting`                      | Global Docker Image registry                                                                                         | `This is the <environment> environment!`                                                        |


**Note**: If upgrading or otherwise changing the ConfigMap, the pods will need to be recreated to retrieve the updated value.

TODO: Documentation on relevant variables.

