![otwld ollama helm chart banner](./banner.png)

![GitHub License](https://img.shields.io/github/license/otwld/ollama-helm)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/ollama-helm)](https://artifacthub.io/packages/helm/ollama-helm/ollama)
[![Helm Lint and Test](https://github.com/otwld/ollama-helm/actions/workflows/ci.yaml/badge.svg?branch=main)](https://github.com/otwld/ollama-helm/actions/workflows/ci.yaml)
[![Discord](https://img.shields.io/badge/Discord-OTWLD-blue?logo=discord&logoColor=white)](https://discord.gg/U24mpqTynB)

[Ollama](https://ollama.ai/), get up and running with large language models, locally.

This Community Chart is for deploying [Ollama](https://github.com/ollama/ollama).

## Requirements

- Kubernetes: `>= 1.16.0-0` for **CPU only**

- Kubernetes: `>= 1.26.0-0` for **GPU** stable support (NVIDIA and AMD)

*Not all GPUs are currently supported with ollama (especially with AMD)*

## Deploying Ollama chart

To install the `ollama` chart in the `ollama` namespace:

> [!IMPORTANT]  
> We are migrating the registry from https://otwld.github.io/ollama-helm/ url to OTWLD Helm central
> registry https://helm.otwld.com/
> Please update your Helm registry accordingly.

```console
helm repo add otwld https://helm.otwld.com/
helm repo update
helm install ollama otwld/ollama --namespace ollama --create-namespace
```

## Upgrading Ollama chart

First please read the [release notes](https://github.com/ollama/ollama/releases) of Ollama to make sure there are no
backwards incompatible changes.

Make adjustments to your values as needed, then run `helm upgrade`:

```console
# -- This pulls the latest version of the ollama chart from the repo.
helm repo update
helm upgrade ollama otwld/ollama --namespace ollama --values values.yaml
```

## Uninstalling Ollama chart

To uninstall/delete the `ollama` deployment in the `ollama` namespace:

```console
helm delete ollama --namespace ollama
```

Substitute your values if they differ from the examples. See `helm delete --help` for a full reference on `delete`
parameters and flags.

## Interact with Ollama

- **Ollama documentation can be found [HERE](https://github.com/ollama/ollama/tree/main/docs)**
- Interact with RESTful API: [Ollama API](https://github.com/ollama/ollama/blob/main/docs/api.md)
- Interact with official clients libraries: [ollama-js](https://github.com/ollama/ollama-js#custom-client)
  and [ollama-python](https://github.com/ollama/ollama-python#custom-client)
- Interact with langchain: [langchain-js](https://github.com/ollama/ollama/blob/main/docs/tutorials/langchainjs.md)
  and [langchain-python](https://github.com/ollama/ollama/blob/main/docs/tutorials/langchainpy.md)

## Examples

- **It's highly recommended to run an updated version of Kubernetes for deploying ollama with GPU**

### Basic values.yaml example with GPU and two models pulled at startup

```
ollama:
  gpu:
    # -- Enable GPU integration
    enabled: true
    
    # -- GPU type: 'nvidia' or 'amd'
    type: 'nvidia'
    
    # -- Specify the number of GPU to 1
    number: 1
   
  # -- List of models to pull at container startup
  models:
    pull:
      - mistral
      - llama2
```

---

### Basic values.yaml example with Ingress

```
ollama:
  models:
    pull:
      - llama2
  
ingress:
  enabled: true
  hosts:
  - host: ollama.domain.lan
    paths:
      - path: /
        pathType: Prefix
```

- *API is now reachable at `ollama.domain.lan`*

---

### Create and run model from template

```
ollama:
  models:
    create:
      - name: llama3.1-ctx32768
        template: |
          FROM llama3.1
          PARAMETER num_ctx 32768
    run:
      - llama3.1-ctx32768
```

## Upgrading from 0.X.X to 1.X.X

The version 1.X.X introduces the ability to load models in memory at startup, the values have been changed.

Please change `ollama.models` to `ollama.models.pull` to avoid errors before upgrading:

```yaml
ollama:
  models:
    - mistral
    - llama2
```

To:

```yaml
ollama:
  models:
    pull:
      - mistral
      - llama2
```

## Helm Values

- See [values.yaml](values.yaml) to see the Chart's default values.

| Key                                        | Type   | Default                        | Description                                                                                                                                                                                                                                                                                                                                               |
|--------------------------------------------|--------|--------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| affinity                                   | object | `{}`                           | Affinity for pod assignment                                                                                                                                                                                                                                                                                                                               |
| autoscaling.enabled                        | bool   | `false`                        | Enable autoscaling                                                                                                                                                                                                                                                                                                                                        |
| autoscaling.maxReplicas                    | int    | `100`                          | Number of maximum replicas                                                                                                                                                                                                                                                                                                                                |
| autoscaling.minReplicas                    | int    | `1`                            | Number of minimum replicas                                                                                                                                                                                                                                                                                                                                |
| autoscaling.targetCPUUtilizationPercentage | int    | `80`                           | CPU usage to target replica                                                                                                                                                                                                                                                                                                                               |
| deployment.labels                          | object | `{}`                           | Labels to add to the deployment                                                                                                                                                                                                                                                                                                                           |
| extraArgs                                  | list   | `[]`                           | Additional arguments on the output Deployment definition.                                                                                                                                                                                                                                                                                                 |
| extraEnv                                   | list   | `[]`                           | Additional environments variables on the output Deployment definition. For extra OLLAMA env, please refer to https://github.com/ollama/ollama/blob/main/envconfig/config.go                                                                                                                                                                               |
| extraEnvFrom                               | list   | `[]`                           | Additionl environment variables from external sources (like ConfigMap)                                                                                                                                                                                                                                                                                    |
| extraObjects                               | list   | `[]`                           | Extra K8s manifests to deploy                                                                                                                                                                                                                                                                                                                             |
| fullnameOverride                           | string | `""`                           | String to fully override template                                                                                                                                                                                                                                                                                                                         |
| hostIPC                                    | bool   | `false`                        | Use the host’s ipc namespace.                                                                                                                                                                                                                                                                                                                             |
| hostNetwork                                | bool   | `false`                        | Use the host's network namespace.                                                                                                                                                                                                                                                                                                                         |
| hostPID                                    | bool   | `false`                        | Use the host’s pid namespace                                                                                                                                                                                                                                                                                                                              |
| image.pullPolicy                           | string | `"IfNotPresent"`               | Docker pull policy                                                                                                                                                                                                                                                                                                                                        |
| image.repository                           | string | `"ollama/ollama"`              | Docker image registry                                                                                                                                                                                                                                                                                                                                     |
| image.tag                                  | string | `""`                           | Docker image tag, overrides the image tag whose default is the chart appVersion.                                                                                                                                                                                                                                                                          |
| imagePullSecrets                           | list   | `[]`                           | Docker registry secret names as an array                                                                                                                                                                                                                                                                                                                  |
| ingress.annotations                        | object | `{}`                           | Additional annotations for the Ingress resource.                                                                                                                                                                                                                                                                                                          |
| ingress.className                          | string | `""`                           | IngressClass that will be used to implement the Ingress (Kubernetes 1.18+)                                                                                                                                                                                                                                                                                |
| ingress.enabled                            | bool   | `false`                        | Enable ingress controller resource                                                                                                                                                                                                                                                                                                                        |
| ingress.hosts[0].host                      | string | `"ollama.local"`               |                                                                                                                                                                                                                                                                                                                                                           |
| ingress.hosts[0].paths[0].path             | string | `"/"`                          |                                                                                                                                                                                                                                                                                                                                                           |
| ingress.hosts[0].paths[0].pathType         | string | `"Prefix"`                     |                                                                                                                                                                                                                                                                                                                                                           |
| ingress.tls                                | list   | `[]`                           | The tls configuration for hostnames to be covered with this ingress record.                                                                                                                                                                                                                                                                               |
| initContainers                             | list   | `[]`                           | Init containers to add to the pod                                                                                                                                                                                                                                                                                                                         |
| knative.annotations                        | object | `{}`                           | Knative service annotations                                                                                                                                                                                                                                                                                                                               |
| knative.containerConcurrency               | int    | `0`                            | Knative service container concurrency                                                                                                                                                                                                                                                                                                                     |
| knative.enabled                            | bool   | `false`                        | Enable Knative integration                                                                                                                                                                                                                                                                                                                                |
| knative.idleTimeoutSeconds                 | int    | `300`                          | Knative service idle timeout seconds                                                                                                                                                                                                                                                                                                                      |
| knative.responseStartTimeoutSeconds        | int    | `300`                          | Knative service response start timeout seconds                                                                                                                                                                                                                                                                                                            |
| knative.timeoutSeconds                     | int    | `300`                          | Knative service timeout seconds                                                                                                                                                                                                                                                                                                                           |
| lifecycle                                  | object | `{}`                           | Lifecycle for pod assignment (override ollama.models startup pull/run)                                                                                                                                                                                                                                                                                    |
| livenessProbe.enabled                      | bool   | `true`                         | Enable livenessProbe                                                                                                                                                                                                                                                                                                                                      |
| livenessProbe.failureThreshold             | int    | `6`                            | Failure threshold for livenessProbe                                                                                                                                                                                                                                                                                                                       |
| livenessProbe.initialDelaySeconds          | int    | `60`                           | Initial delay seconds for livenessProbe                                                                                                                                                                                                                                                                                                                   |
| livenessProbe.path                         | string | `"/"`                          | Request path for livenessProbe                                                                                                                                                                                                                                                                                                                            |
| livenessProbe.periodSeconds                | int    | `10`                           | Period seconds for livenessProbe                                                                                                                                                                                                                                                                                                                          |
| livenessProbe.successThreshold             | int    | `1`                            | Success threshold for livenessProbe                                                                                                                                                                                                                                                                                                                       |
| livenessProbe.timeoutSeconds               | int    | `5`                            | Timeout seconds for livenessProbe                                                                                                                                                                                                                                                                                                                         |
| nameOverride                               | string | `""`                           | String to partially override template  (will maintain the release name)                                                                                                                                                                                                                                                                                   |
| namespaceOverride                          | string | `""`                           | String to fully override namespace                                                                                                                                                                                                                                                                                                                        |
| nodeSelector                               | object | `{}`                           | Node labels for pod assignment.                                                                                                                                                                                                                                                                                                                           |
| ollama.gpu.enabled                         | bool   | `false`                        | Enable GPU integration                                                                                                                                                                                                                                                                                                                                    |
| ollama.gpu.mig.devices                     | object | `{}`                           | Specify the mig devices and the corresponding number                                                                                                                                                                                                                                                                                                      |
| ollama.gpu.mig.enabled                     | bool   | `false`                        | Enable multiple mig devices If enabled you will have to specify the mig devices If enabled is set to false this section is ignored                                                                                                                                                                                                                        |
| ollama.gpu.number                          | int    | `1`                            | Specify the number of GPU If you use MIG section below then this parameter is ignored                                                                                                                                                                                                                                                                     |
| ollama.gpu.nvidiaResource                  | string | `"nvidia.com/gpu"`             | only for nvidia cards; change to (example) 'nvidia.com/mig-1g.10gb' to use MIG slice                                                                                                                                                                                                                                                                      |
| ollama.gpu.type                            | string | `"nvidia"`                     | GPU type: 'nvidia' or 'amd' If 'ollama.gpu.enabled', default value is nvidia If set to 'amd', this will add 'rocm' suffix to image tag if 'image.tag' is not override This is due cause AMD and CPU/CUDA are different images                                                                                                                             |
| ollama.insecure                            | bool   | `false`                        | Add insecure flag for pulling at container startup                                                                                                                                                                                                                                                                                                        |
| ollama.models.create                       | list   | `[]`                           | List of models to create at container startup, there are two options 1. Create a raw model 2. Load a model from configMaps, configMaps must be created before and are loaded as volume in "/models" directory. create:  - name: llama3.1-ctx32768    configMapRef: my-configmap    configMapKeyRef: configmap-key  - name: llama3.1-ctx32768    template: |      FROM llama3.1      PARAMETER num_ctx 32768 |
| ollama.models.pull                         | list   | `[]`                           | List of models to pull at container startup The more you add, the longer the container will take to start if models are not present pull:  - llama2  - mistral                                                                                                                                                                                            |
| ollama.models.run                          | list   | `[]`                           | List of models to load in memory at container startup run:  - llama2  - mistral                                                                                                                                                                                                                                                                           |
| ollama.mountPath                           | string | `""`                           | Override ollama-data volume mount path, default: "/root/.ollama"                                                                                                                                                                                                                                                                                          |
| ollama.port                                | int    | `11434`                        |                                                                                                                                                                                                                                                                                                                                                           |
| persistentVolume.accessModes               | list   | `["ReadWriteOnce"]`            | Ollama server data Persistent Volume access modes Must match those of existing PV or dynamic provisioner Ref: http://kubernetes.io/docs/user-guide/persistent-volumes/                                                                                                                                                                                    |
| persistentVolume.annotations               | object | `{}`                           | Ollama server data Persistent Volume annotations                                                                                                                                                                                                                                                                                                          |
| persistentVolume.enabled                   | bool   | `false`                        | Enable persistence using PVC                                                                                                                                                                                                                                                                                                                              |
| persistentVolume.existingClaim             | string | `""`                           | If you'd like to bring your own PVC for persisting Ollama state, pass the name of the created + ready PVC here. If set, this Chart will not create the default PVC. Requires server.persistentVolume.enabled: true                                                                                                                                        |
| persistentVolume.size                      | string | `"30Gi"`                       | Ollama server data Persistent Volume size                                                                                                                                                                                                                                                                                                                 |
| persistentVolume.storageClass              | string | `""`                           | Ollama server data Persistent Volume Storage Class If defined, storageClassName: <storageClass> If set to "-", storageClassName: "", which disables dynamic provisioning If undefined (the default) or set to null, no storageClassName spec is set, choosing the default provisioner.  (gp2 on AWS, standard on GKE, AWS & OpenStack)                    |
| persistentVolume.subPath                   | string | `""`                           | Subdirectory of Ollama server data Persistent Volume to mount Useful if the volume's root directory is not empty                                                                                                                                                                                                                                          |
| persistentVolume.volumeMode                | string | `""`                           | Ollama server data Persistent Volume Binding Mode If defined, volumeMode: <volumeMode> If empty (the default) or set to null, no volumeBindingMode spec is set, choosing the default mode.                                                                                                                                                                |
| persistentVolume.volumeName                | string | `""`                           | Pre-existing PV to attach this claim to Useful if a CSI auto-provisions a PV for you and you want to always reference the PV moving forward                                                                                                                                                                                                               |
| podAnnotations                             | object | `{}`                           | Map of annotations to add to the pods                                                                                                                                                                                                                                                                                                                     |
| podLabels                                  | object | `{}`                           | Map of labels to add to the pods                                                                                                                                                                                                                                                                                                                          |
| podSecurityContext                         | object | `{}`                           | Pod Security Context                                                                                                                                                                                                                                                                                                                                      |
| priorityClassName                          | string | `""`                           | Priority Class Name                                                                                                                                                                                                                                                                                                                                       |
| readinessProbe.enabled                     | bool   | `true`                         | Enable readinessProbe                                                                                                                                                                                                                                                                                                                                     |
| readinessProbe.failureThreshold            | int    | `6`                            | Failure threshold for readinessProbe                                                                                                                                                                                                                                                                                                                      |
| readinessProbe.initialDelaySeconds         | int    | `30`                           | Initial delay seconds for readinessProbe                                                                                                                                                                                                                                                                                                                  |
| readinessProbe.path                        | string | `"/"`                          | Request path for readinessProbe                                                                                                                                                                                                                                                                                                                           |
| readinessProbe.periodSeconds               | int    | `5`                            | Period seconds for readinessProbe                                                                                                                                                                                                                                                                                                                         |
| readinessProbe.successThreshold            | int    | `1`                            | Success threshold for readinessProbe                                                                                                                                                                                                                                                                                                                      |
| readinessProbe.timeoutSeconds              | int    | `3`                            | Timeout seconds for readinessProbe                                                                                                                                                                                                                                                                                                                        |
| replicaCount                               | int    | `1`                            | Number of replicas                                                                                                                                                                                                                                                                                                                                        |
| resources.limits                           | object | `{}`                           | Pod limit                                                                                                                                                                                                                                                                                                                                                 |
| resources.requests                         | object | `{}`                           | Pod requests                                                                                                                                                                                                                                                                                                                                              |
| runtimeClassName                           | string | `""`                           | Specify runtime class                                                                                                                                                                                                                                                                                                                                     |
| securityContext                            | object | `{}`                           | Container Security Context                                                                                                                                                                                                                                                                                                                                |
| service.annotations                        | object | `{}`                           | Annotations to add to the service                                                                                                                                                                                                                                                                                                                         |
| service.labels                             | object | `{}`                           | Labels to add to the service                                                                                                                                                                                                                                                                                                                              |
| service.loadBalancerIP                     | string | `nil`                          | Load Balancer IP address                                                                                                                                                                                                                                                                                                                                  |
| service.nodePort                           | int    | `31434`                        | Service node port when service type is 'NodePort'                                                                                                                                                                                                                                                                                                         |
| service.port                               | int    | `11434`                        | Service port                                                                                                                                                                                                                                                                                                                                              |
| service.type                               | string | `"ClusterIP"`                  | Service type                                                                                                                                                                                                                                                                                                                                              |
| serviceAccount.annotations                 | object | `{}`                           | Annotations to add to the service account                                                                                                                                                                                                                                                                                                                 |
| serviceAccount.automount                   | bool   | `true`                         | Automatically mount a ServiceAccount's API credentials?                                                                                                                                                                                                                                                                                                   |
| serviceAccount.create                      | bool   | `true`                         | Specifies whether a service account should be created                                                                                                                                                                                                                                                                                                     |
| serviceAccount.name                        | string | `""`                           | The name of the service account to use. If not set and create is true, a name is generated using the fullname template                                                                                                                                                                                                                                    |
| terminationGracePeriodSeconds              | int    | `120`                          | Wait for a grace period                                                                                                                                                                                                                                                                                                                                   |
| tests.labels                               | object | `{}`                           | Labels to add to the tests                                                                                                                                                                                                                                                                                                                                |
| tolerations                                | list   | `[]`                           | Tolerations for pod assignment                                                                                                                                                                                                                                                                                                                            |
| topologySpreadConstraints                  | object | `{}`                           | Topology Spread Constraints for pod assignment                                                                                                                                                                                                                                                                                                            |
| updateStrategy.type                        | string | `"Recreate"`                   | Deployment strategy can be "Recreate" or "RollingUpdate". Default is Recreate                                                                                                                                                                                                                                                                             |
| volumeMounts                               | list   | `[]`                           | Additional volumeMounts on the output Deployment definition.                                                                                                                                                                                                                                                                                              |
| volumes                                    | list   | `[]`                           | Additional volumes on the output Deployment definition.                                                                                                                                                                                                                                                                                                   |

----------------------------------------------

## Core team

<table>
    <tr>
       <td align="center">
            <a href="https://github.com/jdetroyes"
                ><img
                    src="https://github.com/jdetroyes.png?size=200"
                    width="50"
                    style="margin-bottom: -4px; border-radius: 8px;"
                    alt="Jean Baptiste Detroyes"
                /><br /><b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Jean Baptiste&nbsp;Detroyes&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</b></a
            >
            <div style="margin-top: 4px">
                <a href="https://github.com/jdetroyes" title="Github"
                    ><img
                        width="16"
                        src="https://raw.githubusercontent.com/MarsiBarsi/readme-icons/main/github.svg"
                /></a>
                <a
                    href="mailto:jdetroyes@otwld.com"
                    title="Email"
                    ><img
                        width="16"
                        src="https://raw.githubusercontent.com/MarsiBarsi/readme-icons/main/send.svg"
                /></a>
            </div>
        </td>
       <td align="center">
            <a href="https://github.com/ntrehout"
                ><img
                    src="https://github.com/ntrehout.png?size=200"
                    width="50"
                    style="margin-bottom: -4px; border-radius: 8px;"
                    alt="Jean Baptiste Detroyes"
                /><br /><b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nathan&nbsp;Tréhout&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</b></a
            >
            <div style="margin-top: 4px">
                <a href="https://x.com/n_trehout" title="Twitter"
                    ><img
                        width="16"
                        src="https://raw.githubusercontent.com/MarsiBarsi/readme-icons/main/twitter.svg"
                /></a>
                <a href="https://github.com/ntrehout" title="Github"
                    ><img
                        width="16"
                        src="https://raw.githubusercontent.com/MarsiBarsi/readme-icons/main/github.svg"
                /></a>
                <a
                    href="mailto:ntrehout@otwld.com"
                    title="Email"
                    ><img
                        width="16"
                        src="https://raw.githubusercontent.com/MarsiBarsi/readme-icons/main/send.svg"
                /></a>
            </div>
        </td>
    </tr>
</table>

## Support

- For questions, suggestions, and discussion about Ollama please refer to
  the [Ollama issue page](https://github.com/ollama/ollama/issues)
- For questions, suggestions, and discussion about this chart please
  visit [Ollama-Helm issue page](https://github.com/otwld/ollama-helm/issues) or join
  our [OTWLD Discord](https://discord.gg/U24mpqTynB)
