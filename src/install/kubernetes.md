# On Kubernetes

This section explains how to install lockc on a Kubernetes cluster with
[helm](https://helm.sh/).

The helm chart is available on [lockc-helm-chart](https://lockc-project.github.io/helm-charts/)
website. Installation with default values can be done with:

```bash
kubectl apply -f https://lockc-project.github.io/helm-charts/namespace.yaml
helm repo add lockc https://lockc-project.github.io/helm-charts/
helm install -n lockc lockc lockc/lockc
```

More info on lockc helm chart installation can be found [here](https://lockc-project.github.io/helm-charts/)
