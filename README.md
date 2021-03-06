# Argo Workflows introduction
This repository contains code examples for Argo Workflows Introduction presentation.

**Official Argo Workflows website** https://argoproj.github.io/workflows/

## Cheatsheet
- argo submit -n namespace --watch filename / kubectl apply -f filename -n namespace
- argo list -n namespace 
- argo logs -n namespace @latest
- argo delete -n namespace --all

## How to run
General Requirements:
- Kubernetes cluster
- Installed argo ([instalation tutorial]((https://argoproj.github.io/argo-workflows/quick-start/#install-argo-workflows)))
- Namespace called `Argo`
- Installed Argo CLI 

### Hello World example

Run with default parameter:

```argo submit -n argo intro/hello-world.yaml --watch```

Run with custom parameter:

```argo submit -n argo intro/hello-world-param.yaml --watch -p name="Joe"``` 

### Hello from config map

Create config map:

```kubectl apply -f k8s-config-map/cofig-map.yaml -n argo```

Run workflow reading config map:

```argo submit -n argo k8s-config-map hello-from-config-map.yaml```

### Steps

Run chained steps:

```argo submit -n argo steps/chained-steps.yaml --watch --serviceaccount argo```

Run dynamic parallelization:

```argo submit -n argo steps/multiple-steps.yaml --watch --serviceaccount argo```

### Argo events
This step requires creating topic on GCP and secret in K8s cluster (https://argoproj.github.io/argo-events/eventsources/gcp-pubsub/)

Create EventSource:

```kubectl apply -f events/pub-sub-event-source.yaml -n argo-events```

Deploy Sensor:

```kubectl apply -f events/pub-sub-logger.yaml -n argo-events```

How to trigger:
- publish message to the topic