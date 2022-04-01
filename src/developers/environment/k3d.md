# k3d

## Setting up k3d

To install k3d, follow the [official installation instructions](https://k3d.io/#installation).

After that, set up a cluster with multiple nodes. Example command for a cluster
with 2 servers and 2 agents:

```bash
k3d cluster create mycluster -s 2 -a 2
```

Mount BPFFS on every k3d node:

```bash
docker ps | awk '/k3d/ { print $1; }' | xargs -i docker exec -i {} mount bpffs /sys/fs/bpf -t bpf
```

After those commands, you should have a working cluster, which you can check
with `kubectl cluster-info`.

## Building and innstalling lockc

To build lockc container image, we have to go to the main directory in lockc
git repository:

```bash
# Go to the main directory of lockc sources
cd ../../..
export IMAGE_NAME=$(uuidgen)
docker build -t ttl.sh/${IMAGE_NAME}:30m .
docker push ttl.sh/${IMAGE_NAME}:30m
```

Then we need to go to the helm-charts git repository:

```bash
# Go to the main directory of helm-charts sources
cd ../helm-charts
kubectl apply -f https://lockc-project.github.io/helm-charts/namespace.yaml
helm install lockc charts/lockc/ --namespace lockc \
    --set lockcd.image.repository=ttl.sh/${IMAGE_NAME} \
    --set lockcd.image.tag=30m \
    --set lockcd.log.level=debug
```

Then wait until the `lockcd` DaemonSet is ready:

```bash
# Go to the main directory of helm-charts sources
cd ../helm-charts
kubectl apply -f https://lockc-project.github.io/helm-charts/namespace.yaml
helm install lockc charts/lockc/ --namespace lockc \
    --set lockcd.image.repository=ttl.sh/${IMAGE_NAME} \
    --set lockcd.image.tag=30m \
    --set lockcd.log.level=debug
```