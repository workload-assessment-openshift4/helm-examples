# Application
This chart will deploy a simple Spring Boot application.

## Installation Guide

1. After checking out the repo, Run the following command in *this* directory: 

`helm install <name-of-app> . --set domain <domain>  --set configmap.greeting=<message>`

**Example**: 
`helm install test . --set domain apps.cluster-ccf2.sandbox801.opentlc.com  --set configmap.greeting="Hello!"`

TODO: Maybe add possible variables and what they mean/affect?

