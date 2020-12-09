# Application
This chart will deploy a simple Spring Boot application.

## Installation Guide

After checking out the repo, run the following command in *this* directory: 

`helm install <name-of-app> .`

## Parameters

| Parameter                                 | Description                                                                                                          | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `configmap.greeting`                      | Greeting that will be displayed by the Spring application.                                                           | `This is amazing!`                     |


**Note**: If upgrading or otherwise changing the ConfigMap, the pods will need to be recreated to retrieve the updated value.

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

#### configmap.yaml
The `configmap.yaml` is used to create a ConfigMap resource. If there are any environment variables that you need to need to make avaialble to any of your applications or services you can define them in here.

#### values.yaml file

The `values.yaml` file provides us access to values passed into the Helm templates. Within this file we can customize values our Openshift resources will utilize upon installation of the chart into the cluster. These values can also be overriden with the `--set` command upon installation of a chart.

This file is the default file when performing helm installations; if you have any additional variables or variables you want to overwrite you can create another values file and pass it as a parameter when running the helm command.

The `values.yaml` file provides us access to values passed into the Helm chart. Within this file we can customize values our application will utilize upon deployment. An advantage of the `values.yaml` file is that its values can be referenced within other configuration files within the chart. This allows us to parameterize fields in our other files saving time and providing clarity during the development process. 

=== Creating Liveness and Readiness Probes in Helm Charts
In software systems, components can become unhealthy due to transient issues (such as temporary connectivity loss), configuration errors, or problems with external dependencies. OpenShift Container Platform applications have a number of options to detect and handle unhealthy containers.

*Liveness Probe*

A liveness probe checks if the container in which it is configured is still running. If the liveness probe fails, the kubelet kills the container, which will be subjected to its restart policy.

*When to use a Liveness Probe*

If you'd like your container to be killed and restarted if a probe fails, then specify a liveness probe, and specify a restartPolicy of Always or OnFailure.

If the process in your container is able to crash on its own whenever it encounters an issue or becomes unhealthy, you do not necessarily need a liveness probe; the kubelet will automatically perform the correct action in accordance with the Pod's restartPolicy.

*Readiness Probe*

A readiness probe determines if a container is ready to service requests. If the readiness probe fails a container, the endpoints controller ensures the container has its IP address removed from the endpoints of all services. A readiness probe can be used to signal to the endpoints controller that even though a container is running, it should not receive any traffic from a proxy.

*When to use a Readiness Probe*

If you'd like to start sending traffic to a Pod only when a probe succeeds, specify a readiness probe. In this case, the readiness probe might be the same as the liveness probe, but the existence of the readiness probe in the spec means that the Pod will start without receiving any traffic and only start receiving traffic after the probe starts succeeding. If your container needs to work on loading large data, configuration files, or migrations during startup, specify a readiness probe.

If you want your container to be able to take itself down for maintenance, you can specify a readiness probe that checks an endpoint specific to readiness that is different from the liveness probe.

*Initializing Probes in Helm Chart* 

. Navigate to the _values.yaml_ in your new chart's  directory.
    
+
```yaml
livenessProbe:
  enabled: true
  initialDelaySeconds: 120
  periodSeconds: 10
  timeoutSeconds: 5
  failureThreshold: 6
  successThreshold: 1
readinessProbe:
  enabled: true
  initialDelaySeconds: 30
  periodSeconds: 10
  timeoutSeconds: 5
  failureThreshold: 6
  successThreshold: 1
```


    initialDelaySeconds:: Tells the kubelet that it should wait 5 seconds before performing the first probe.

    periodSeconds:: Specifies that the kubelet controller should perform a liveness/readiness probe every 5 seconds.

    timeoutSeconds:: Number of seconds after which the probe times out. Defaults to 1 second. Minimum value is 1.

    failureThreshold:: When a probe fails, Kubernetes will try failureThreshold times before giving up. Giving up in case of liveness probe means restarting the container. In case of readiness probe the Pod will be marked Unready. Defaults to 3. Minimum value is 1.

    successThreshold:: Minimum consecutive successes for the probe to be considered successful after having failed. Defaults to 1. Must be 1 for liveness and startup Probes. Minimum value is 1.

. Navigate to the _deployments.yaml_ file in your templates directory.

. Reference configurations made in _values.yaml_ file in the Liveness and Readiness fields
+
```yaml
        {{- if .Values.livenessProbe.enabled }}
          livenessProbe:
            initialDelaySeconds: {{ .Values.livenessProbe.initialDelaySeconds }}
            periodSeconds: {{ .Values.livenessProbe.periodSeconds }}
            timeoutSeconds: {{ .Values.livenessProbe.timeoutSeconds }}
            successThreshold: {{ .Values.livenessProbe.successThreshold }}
            failureThreshold: {{ .Values.livenessProbe.failureThreshold }}
            httpGet:
              path: {{ .Values.handlerPath }}
              port: http
        {{- end }}
        {{- if .Values.readinessProbe.enabled }}
          readinessProbe:
            initialDelaySeconds: {{ .Values.readinessProbe.initialDelaySeconds }}
            periodSeconds: {{ .Values.readinessProbe.periodSeconds }}
            timeoutSeconds: {{ .Values.readinessProbe.timeoutSeconds }}
            successThreshold: {{ .Values.readinessProbe.successThreshold }}
            failureThreshold: {{ .Values.readinessProbe.failureThreshold }}
            httpGet:
              path: {{ .Values.handlerPath }}
              port: http
        {{- end }}
```
