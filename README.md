# Ollama Helm Chart

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/ollama-helm)](https://artifacthub.io/packages/search?repo=ollama-helm)


[Ollama](https://ollama.ai/), get up and running with large language models, locally.

This Chart is for deploying [Ollama](https://github.com/ollama/ollama). 

## Requirements

- Kubernetes: `>= 1.16.0-0` for **CPU only**

- Kubernetes: `>= 1.26.0-0` for **GPU** stable support

## Deploying Ollama chart

To install the `ollama` chart in the `ollama` namespace:

```console
helm repo add ollama-helm https://otwld.github.io/ollama-helm/
helm repo update
helm install ollama ollama-helm/ollama --namespace ollama
```

## Upgrading Ollama chart

First please read the [release notes](https://github.com/ollama/ollama/releases) of Ollama to make sure there are no backwards incompatible changes.

Make adjustments to your values as needed, then run `helm upgrade`:

```console
# -- This pulls the latest version of the ollama chart from the repo.
helm repo update
helm upgrade ollama ollama-helm/ollama --namespace ollama --values values.yaml
```

## Uninstalling Ollama chart

To uninstall/delete the `ollama` deployment in the `ollama` namespace:

```console
helm delete ollama --namespace ollama
```

Substitute your values if they differ from the examples. See `helm delete --help` for a full reference on `delete` parameters and flags.


## Interact with Ollama

- **Ollama documentation can be found [HERE](https://github.com/ollama/ollama/tree/main/docs)**
- Interact with RESTful API: [Ollama API](https://github.com/ollama/ollama/blob/main/docs/api.md) 
- Interact with official clients libraries: [ollama-js](https://github.com/ollama/ollama-js#custom-client) and [ollama-python](https://github.com/ollama/ollama-python#custom-client)
- Interact with langchain: [langchain-js](https://github.com/ollama/ollama/blob/main/docs/tutorials/langchainjs.md) and [langchain-python](https://github.com/ollama/ollama/blob/main/docs/tutorials/langchainpy.md)


## Examples
- **It's highly recommended to run an updated version of Kubernetes for deploying ollama with GPU**

### Basic values.yaml example with GPU and default model
```
ollama:
  gpu:
    # -- Enable GPU integration
    enabled: true
    # -- Specify the number of GPU to 2
    number: 2
  # -- Set default model to mistral
  defaultModel: "mistral"
```
---
### Basic values.yaml example with Ingress
```
ollama:
  defaultModel: "llama2"
ingress:
  enabled: true
    hosts:
    - host: ollama.domain.lan
      paths:
        - path: /
          pathType: Prefix
```

- *APi is now reachable at `ollama.domain.lan`*

## Helm Values

- See [values.yaml](values.yaml) to see the Chart's default values.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity for pod assignment |
| autoscaling.enabled | bool | `false` | Enable autoscaling |
| autoscaling.maxReplicas | int | `100` | Number of maximum replicas |
| autoscaling.minReplicas | int | `1` | Number of minimum replicas |
| autoscaling.targetCPUUtilizationPercentage | int | `80` | CPU usage to target replica |
| extraArgs | list | `[]` | Additional arguments on the output Deployment definition. |
| fullnameOverride | string | `""` | String to fully override template |
| image.pullPolicy | string | `"IfNotPresent"` | Docker pull policy |
| image.repository | string | `"ollama/ollama"` | Docker image registry |
| image.tag | string | `""` | Docker image tag, overrides the image tag whose default is the chart appVersion. |
| imagePullSecrets | list | `[]` | Docker registry secret names as an array |
| ingress.annotations | object | `{}` | Additional annotations for the Ingress resource. |
| ingress.className | string | `""` | IngressClass that will be used to implement the Ingress (Kubernetes 1.18+) |
| ingress.enabled | bool | `false` | Enable ingress controller resource |
| ingress.hosts[0].host | string | `"ollama.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"Prefix"` |  |
| ingress.tls | list | `[]` | The tls configuration for hostnames to be covered with this ingress record. |
| livenessProbe.enabled | bool | `true` | Enable livenessProbe |
| livenessProbe.failureThreshold | int | `6` | Failure threshold for livenessProbe |
| livenessProbe.initialDelaySeconds | int | `60` | Initial delay seconds for livenessProbe |
| livenessProbe.path | string | `"/"` | Request path for livenessProbe |
| livenessProbe.periodSeconds | int | `10` | Period seconds for livenessProbe |
| livenessProbe.successThreshold | int | `1` | Success threshold for livenessProbe |
| livenessProbe.timeoutSeconds | int | `5` | Timeout seconds for livenessProbe |
| nameOverride | string | `""` | String to partially override template  (will maintain the release name) |
| nodeSelector | object | `{}` | Node labels for pod assignment. |
| ollama.defaultModel | string | `"llama2"` | Default model to serve, if not set, no model will be served at container startup |
| ollama.gpu.enabled | bool | `false` | Enable GPU integration |
| ollama.gpu.number | int | `1` | Specify the number of GPU |
| persistentVolume.accessModes | list | `["ReadWriteOnce"]` | Ollama server data Persistent Volume access modes Must match those of existing PV or dynamic provisioner Ref: http://kubernetes.io/docs/user-guide/persistent-volumes/ |
| persistentVolume.annotations | object | `{}` | Ollama server data Persistent Volume annotations |
| persistentVolume.enabled | bool | `false` | Enable persistence using PVC |
| persistentVolume.existingClaim | string | `""` | If you'd like to bring your own PVC for persisting Ollama state, pass the name of the created + ready PVC here. If set, this Chart will not create the default PVC. Requires server.persistentVolume.enabled: true |
| persistentVolume.size | string | `"30Gi"` | Ollama server data Persistent Volume size |
| persistentVolume.storageClass | string | `""` | Ollama server data Persistent Volume Storage Class If defined, storageClassName: <storageClass> If set to "-", storageClassName: "", which disables dynamic provisioning If undefined (the default) or set to null, no storageClassName spec is set, choosing the default provisioner.  (gp2 on AWS, standard on GKE, AWS & OpenStack) |
| persistentVolume.subPath | string | `""` | Subdirectory of Ollama server data Persistent Volume to mount Useful if the volume's root directory is not empty |
| persistentVolume.volumeMode | string | `""` | Ollama server data Persistent Volume Binding Mode If defined, volumeMode: <volumeMode> If empty (the default) or set to null, no volumeBindingMode spec is set, choosing the default mode. |
| podAnnotations | object | `{}` | Map of annotations to add to the pods |
| podLabels | object | `{}` | Map of labels to add to the pods |
| podSecurityContext | object | `{}` | Pod Security Context |
| readinessProbe.enabled | bool | `true` | Enable readinessProbe |
| readinessProbe.failureThreshold | int | `6` | Failure threshold for readinessProbe |
| readinessProbe.initialDelaySeconds | int | `30` | Initial delay seconds for readinessProbe |
| readinessProbe.path | string | `"/"` | Request path for readinessProbe |
| readinessProbe.periodSeconds | int | `5` | Period seconds for readinessProbe |
| readinessProbe.successThreshold | int | `1` | Success threshold for readinessProbe |
| readinessProbe.timeoutSeconds | int | `3` | Timeout seconds for readinessProbe |
| replicaCount | int | `1` | Number of replicas |
| resources.limits | object | `{"cpu":"4000m","memory":"8192Mi"}` | Pod limit |
| resources.requests | object | `{"cpu":"2000m","memory":"4096Mi"}` | Pod requests |
| runtimeClassName | string | `""` | Specify runtime class |
| securityContext | object | `{}` | Container Security Context |
| service.port | int | `11434` | Service port |
| service.type | string | `"ClusterIP"` | Service type |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.automount | bool | `true` | Automatically mount a ServiceAccount's API credentials? |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |
| tolerations | list | `[]` | Tolerations for pod assignment |
| volumeMounts | list | `[]` | Additional volumeMounts on the output Deployment definition. |
| volumes | list | `[]` | Additional volumes on the output Deployment definition. |


## Support

- For questions, suggestions, and discussion about Ollama please refer to the [Ollama issue page](https://github.com/ollama/ollama/issues)
- For questions, suggestions, and discussion about this chart please visite [Ollama-Helm issue page](https://github.com/otwld/ollama-helm/issues)