# Kind Installation

## CLI Tool

```shell
# see https://kind.sigs.k8s.io/docs/user/quick-start#installation
# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-arm64
chmod +x kind
mv kind ~/bin  
```

## Inotify Settings

```shell
# see https://kind.sigs.k8s.io/docs/user/known-issues/#pod-errors-due-to-too-many-open-files
sudo sysctl fs.inotify.max_user_watches=524288
sudo sysctl fs.inotify.max_user_instances=512
```

## Setup

```shell
curl -Lo cluster.yml https://raw.githubusercontent.com/chornberger-c2c/isovalent-cilium-lab/main/kind/cluster.yml
kind create cluster --name cilium --config cluster.yml
```

## Test

```shell
kubectl get nodes
kubectl get all 
kubectl get pods -A
kubectl config current-context
```

## Documentation

https://kind.sigs.k8s.io/docs/user/quick-start/
