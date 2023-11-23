# Ollma

[Ollama](https://ollama.ai/) Get up and running with large language models, locally.

This Chart is for installing [Ollama](https://github.com/jmorganca/ollama).

## Installing Ollama

See the [drone chart installation guide](./docs/install.md).

## Configuring Ollama

### GPU

The drone server deployment can be used to leverage the Kubernetes secrets to configure values like `DRONE_RPC_SECRET` or other source code repository e.g. `DRONE_GITHUB_CLIENT_ID`, `DRONE_GITHUB_CLIENT_SECRET`. Refer to the [Drone server reference](https://docs.drone.io/installation/reference/) for a more complete list of options.

Here is an example that creates the secrets from environment variables `DRONE_RPC_SECRET`, `DRONE_GITHUB_CLIENT_ID` and `DRONE_GITHUB_CLIENT_SECRET`,

```shell
kubectl create secret my-drone-secret generic \
  --from-literal=DRONE_RPC_SECRET=$DRONE_RPC_SECRET \
  --from-literal=DRONE_GITHUB_CLIENT_ID=$DRONE_GITHUB_CLIENT_ID \
  --from-literal=DRONE_GITHUB_CLIENT_SECRET=$DRONE_GITHUB_CLIENT_SECRET 
```

### Helm Values

Once you have created the secret then you can refer to the secret in `values.yaml` like,

```yaml
...
extraSecretNamesForEnvFrom:
  - my-drone-secret
...
```

See [values.yaml](values.yaml) to see the Chart's default values.

To adjust an existing Drone install's configuration:

```console
# If you have a values file:
helm upgrade drone drone/drone --namespace drone --values drone-values.yaml
# If you want to change one value and don't have a values file:
helm upgrade drone drone/drone --namespace drone --reuse-values --set someKey=someVal
```

## Upgrading Drone server

Read the [release notes](https://discourse.drone.io/c/announcements/6) to make sure there are no backwards incompatible changes. Make adjustments to your values as needed, then run `helm upgrade`:

```console
# This pulls the latest version of the drone chart from the repo.
helm repo update
helm upgrade drone drone/drone --namespace drone --values drone-values.yaml
```

## Uninstalling Drone server

To uninstall/delete the `drone` deployment in the `drone` namespace:

```console
helm delete ollama --namespace ollama
```

Substitute your values if they differ from the examples. See `helm delete --help` for a full reference on `delete` parameters and flags.

## Support

- For questions, suggestions, and discussion about Ollama please refer to the [Ollama issue page](https://github.com/jmorganca/ollama/issues)
- For questions, suggestions, and discussion about this chart please visite [Ollama-Helm issue](https://github.com/otwld/ollama-helm/issues)