# Clusternet Hub

## TL;DR

```console
helm repo add clusternet https://clusternet.github.io/charts
helm install my-clusternet-hub clusternet/clusternet-hub
```

## Introduction

`clusternet-hub` is responsible for

- approving cluster registration requests and creating dedicated resources, such as namespaces, serviceaccounts and RBAC
  rules, for each child cluster;
- serving as an **aggregated apiserver (AA)**, which is used to serve as a websocket server that maintain multiple
  active websocket connections from child clusters;
- providing Kubernstes-styled API to redirect/proxy/upgrade requests to each child cluster;
- coordinating and deploying applications to multiple clusters from a single set of APIs;

> :pushpin: :pushpin: Note:
>
> Since `clusternet-hub` is running as an AA, please make sure that parent apiserver could visit the
> `clusternet-hub` service.

## Prerequisites

- Kubernetes 1.18+
- Helm 3.1.0

## Installing the Chart

To install the chart with the release name `my-clusternet-hub`:

```console
helm repo add clusternet https://clusternet.github.io/charts
helm install my-clusternet-hub clusternet/clusternet-hub
```

These commands deploy `clusternet-hub` on the Kubernetes cluster in the default configuration.
The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-clusternet-hub` deployment:

```console
helm delete my-clusternet-hub
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

### Common parameters

| Name                     | Description                                                                             | Value           |
| ------------------------ | --------------------------------------------------------------------------------------- | --------------- |
| `nameOverride`           | String to partially override kafka.fullname                                             | `""`            |
| `fullnameOverride`       | String to fully override kafka.fullname                                                 | `""`            |
| `clusterDomain`          | Default Kubernetes cluster domain                                                       | `cluster.local` |
| `commonLabels`           | Labels to add to all deployed objects                                                   | `{}`            |
| `commonAnnotations`      | Annotations to add to all deployed objects                                              | `{}`            |

### Exposure parameters

| Name                        | Description                                                                               | Value                                                                                                   |
| --------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `replicaCount`              | Specify number of clusternet-hub replicas                                                 | `1`                                                                                                     |
| `serviceAccount.name`       | The name of the ServiceAccount to create                                                  | `"clusternet-hub"`                                                                                      |
| `securePort`                | Port where clusternet-hub will be running                                                 | `443`                                                                                                   |
| `image.registry`            | clusternet-hub image registry                                                             | `ghcr.io`                                                                                               |
| `image.repository`          | clusternet-hub image repository                                                           | `clusternet/clusternet-hub`                                                                             |
| `image.tag`                 | clusternet-hub image tag (immutable tags are recommended)                                 | `v0.5.0`                                                                                                |
| `image.pullPolicy`          | clusternet-hub image pull policy                                                          | `IfNotPresent`                                                                                          |
| `image.pullSecrets`         | Specify docker-registry secret names as an array                                          | `[]`                                                                                                    |
| `extraArgs`                 | Additional command line arguments to pass to clusternet-hub                               | `{"v":4,"feature-gates":"SocketConnection=true,Deployer=true,ShadowAPI=true,FeedInUseProtection=true"}` |
| `resources.limits`          | The resources limits for the container                                                    | `{}`                                                                                                    |
| `resources.requests`        | The requested resources for the container                                                 | `{}`                                                                                                    |
| `nodeSelector`              | Node labels for pod assignment                                                            | `{}`                                                                                                    |
| `priorityClassName`         | Set Priority Class Name to allow priority control over other pods                         | `""`                                                                                                    |
| `tolerations`               | Tolerations for pod assignment                                                            | `[]`                                                                                                    |
| `podAffinityPreset`         | Pod affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`       | `""`                                                                                                    |
| `podAntiAffinityPreset`     | Pod anti-affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`  | `soft`                                                                                                  |
| `nodeAffinityPreset.type`   | Node affinity preset type. Ignored if `affinity` is set. Allowed values: `soft` or `hard` | `""`                                                                                                    |
| `nodeAffinityPreset.key`    | Node label key to match. Ignored if `affinity` is set.                                    | `""`                                                                                                    |
| `nodeAffinityPreset.values` | Node label values to match. Ignored if `affinity` is set.                                 | `[]`                                                                                                    |
| `affinity`                  | Affinity for pod assignment                                                               | `{}`                                                                                                    |
