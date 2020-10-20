# generator-knative-kustomize-skaffold

This generator provides skaffold and kustomize configurations to deploy a Spring Boot application on a Kubernetes cluster with Knative.

It also provides Kubernetes resources for deploying MySQL or Kafka, should the application need it.

Container builds are performed using the [jib maven plugin](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin) with [skaffold](https://skaffold.dev/docs/pipeline-stages/builders/jib/).

## Prerequisites

- [Kustomize](https://kustomize.io/) customizes Kubernetes configuration across different environments (dev, qa, prod) and provide a re-usable library of centralized configuration.
- [Skaffold](https://skaffold.dev/) provides a Kubernetes centric workflow for building, pushing, and deploying your application and it's dependent services and applications.
- [Knative](https://knative.dev/docs/serving/) provides Kubernetes CRDs and controllers, for deployment of serverless containers with autoscaling and scale-to-zero.

## Local Services

### MySQL

If your app depends on a MySQL database, then you must first create a database server instance.

You have two options for this, either using Helm or the Kustomize configuration provided in the `local-services` profile.

Using Kustomize run:

```bash
kubectl create secret generic mysql \
  --from-literal=mysql-root-password=$(echo $RANDOM) \
  --from-literal=mysql-password=$(echo $RANDOM)
skaffold run -p local-mysql
```

Using Helm v3 run:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
skaffold run -p local-mysql-helm
```

### Kafka

If your app depends on a Kafka, then you must first create a Kafka cluster.

You have two options for this, either using Helm or the Kustomize configuration provided in the `local-services` profile.

Using Kustomize run:

```bash
skaffold run -p local-kafka
```

Using Helm v3 run:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
skaffold run -p local-kafka-helm
```

## Run the application

To start the application on a local cluster run:

```bash
skaffold run -p local --default-repo dev.local
```

To start the application on a remote cluster, set `DOCKER_ID` to your Docker Hub ID and then run :

```bash
skaffold run -p local --default-repo $DOCKER_ID
```
