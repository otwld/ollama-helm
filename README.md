# Ollama Helm Chart

[Ollama](https://ollama.ai/) Get up and running with large language models, locally.

This Chart is for installing [Ollama](https://github.com/jmorganca/ollama).

## Requirements

Kubernetes : `>= 1.16.0-0`

## Install Ollama chart

To install the `ollama` chart in the `ollama` namespace:

```console
helm repo add ollama-helm https://otwld.github.io/ollama-helm/
helm repo update
helm install ollama ollama-helm/ollama --namespace ollama
```

## Upgrading Ollama chart

First please read the [release notes](https://github.com/jmorganca/ollama/releases) of Ollama to make sure there are no backwards incompatible changes.

Make adjustments to your values as needed, then run `helm upgrade`:

```console
# This pulls the latest version of the ollama chart from the repo.
helm repo update
helm upgrade ollama ollama-helm/ollama --namespace ollama --values ollama-values.yaml
```

## Uninstalling Ollama chart

To uninstall/delete the `ollama` deployment in the `ollama` namespace:

```console
helm delete ollama --namespace ollama
```

Substitute your values if they differ from the examples. See `helm delete --help` for a full reference on `delete` parameters and flags.

### Helm Values

See [values.yaml](values.yaml) to see the Chart's default values.

## Support

- For questions, suggestions, and discussion about Ollama please refer to the [Ollama issue page](https://github.com/jmorganca/ollama/issues)
- For questions, suggestions, and discussion about this chart please visite [Ollama-Helm issue](https://github.com/otwld/ollama-helm/issues)