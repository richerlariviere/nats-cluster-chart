# NATS-Operator

NATS Operator manages [NATS](https://nats.io/) clusters atop [Kubernetes](http://kubernetes.io), automating their creation and administration.


## TL;DR

```bash
$ helm install .
```

## Introduction

This chart bootstraps a [NATS](https://github.com/bitnami/bitnami-docker-nats) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

Basic
- Kubernetes 1.4+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

Advanced
- Kubernetes 1.10+ with `--feature-gates=PodShareProcessNamespace=true` enabled to benefits from configuration reload feature

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install --name my-release .
```

The command deploys NATS on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the NATS chart and their default values.

| Parameter                            | Description                                                                                  | Default                                         |
|--------------------------------------|----------------------------------------------------------------------------------------------|-------------------------------------------------|
| `rbacEnabled`                        | Switch to enable/disable RBAC for this chart                                                 | `true`                                          |
| `image.registry`                     | NATS image registry                                                                          | `docker.io`                                     |
| `image.repository`                   | NATS image name                                                                              | `connecteverything/nats-operator`               |
| `image.tag`                          | NATS image tag                                                                               | `0.2.3-v1alpha2`                                |
| `image.pullPolicy`                   | Image pull policy                                                                            | `Always`                                        |
| `image.pullSecrets`                  | Specify image pull secrets                                                                   | `nil`                                           |
| `securityContext.enabled`            | Enable security context                                                                      | `true`                                          |
| `securityContext.fsGroup`            | Group ID for the container                                                                   | `1001`                                          |
| `securityContext.runAsUser`          | User ID for the container                                                                    | `1001`                                          |
| `nodeSelector`                       | Node labels for pod assignment                                                               | `nil`                                           |
| `tolerations`                        | Toleration labels for pod assignment                                                         | `nil`                                           |
| `schedulerName`                      | Name of an alternate scheduler                                                               | `nil`                                           |
| `antiAffinity`                       | Anti-affinity for pod assignment (values: soft or hard)                                      | `soft`                                          |
| `podAnnotations`                     | Annotations to be added to pods                                                              | {}                                              |
| `podLabels`                          | Additional labels to be added to pods                                                        | {}                                              |
| `updateStrategy`                     | Replicaset Update strategy                                                                   | `OnDelete`                                      |
| `rollingUpdatePartition`             | Partition for Rolling Update strategy                                                        | `nil`                                           |
| `livenessProbe.enabled`              | Enable liveness probe                                                                        | `true`                                          |
| `livenessProbe.initialDelaySeconds`  | Delay before liveness probe is initiated                                                     | `30`                                            |
| `livenessProbe.periodSeconds`        | How often to perform the probe                                                               | `10`                                            |
| `livenessProbe.timeoutSeconds`       | When the probe times out                                                                     | `5`                                             |
| `livenessProbe.failureThreshold`     | Minimum consecutive failures for the probe to be considered failed after having succeeded.   | `6`                                             |
| `livenessProbe.successThreshold`     | Minimum consecutive successes for the probe to be considered successful after having failed. | `1`                                             |
| `readinessProbe.enabled`             | Enable readiness probe                                                                       | `true`                                          |
| `readinessProbe.initialDelaySeconds` | Delay before readiness probe is initiated                                                    | `5`                                             |
| `readinessProbe.periodSeconds`       | How often to perform the probe                                                               | `10`                                            |
| `readinessProbe.timeoutSeconds`      | When the probe times out                                                                     | `5`                                             |
| `readinessProbe.failureThreshold`    | Minimum consecutive failures for the probe to be considered failed after having succeeded.   | `6`                                             |
| `readinessProbe.successThreshold`    | Minimum consecutive successes for the probe to be considered successful after having failed. | `1`                                             |
| `auth.enabled`                       | Switch to enable/disable client authentication                                               | `true`                                          |
| `tls.enabled`                        | Enable TLS                                                                                   | `false`                                         |
| `tls.serverSecret`                   | Certificates to secure the NATS client connections (type: kubernetes.io/tls)                 | `nil`                                           |
| `tls.routesSecret`                   | Certificates to secure the routes. (type: kubernetes.io/tls)                                 | `nil`                                           |
| `clusterSize`                        | Number of NATS nodes                                                                         | `3`                                             |
| `configReload.enabled`               | Enable configuration reload                                                                  | `false`                                         |
| `configReload.registry`              | Reload configuration name                                                                    | `docker.io`                                     |
| `configReload.repository`            | Reload configuration image name                                                              | `connecteverything/nats-server-config-reloader` |
| `configReload.tag`                   | Reload configuration image tag                                                               | `0.2.2-v1alpha2`                                |

### Example

Here is an example of how to setup a NATS cluster with client authentication.


Edit `config/client-auth.json`
```json
{
  "users": [
    { 
      "username": "my-user",
      "password": "T0pS3cr3t"
    }
  ]
}
```

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. 

```bash
$ helm install --name my-release --set auth.enabled=true .
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml .
```

> **Tip**: You can use the default [values.yaml](values.yaml)
