# Application
This chart will deploy a simple SPring Boot application 

## Installation Guide

1. After checking out the repo, Run the following command in *this* directory: 

`helm install <name-of-app> . --set configmap.greeting="Hello!"`

*Note* if you don't specify a value for `configmap.greeting` a default value will be used

## ConfigMap 

