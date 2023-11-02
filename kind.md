## CLI installation

```shell
# see https://kind.sigs.k8s.io/docs/user/quick-start#installation
# For AMD64 / x86_64
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
# For ARM64
[ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-arm64
chmod +x kind
mv kind ~/bin  
```

## Setup

```shell
curl -Lo cluster.yml https://raw.githubusercontent.com/chornberger-c2c/isovalent-cilium-lab/main/kind/cluster.yml
kind create cluster --name cilium --config cluster.yml
kubectl get nodes
kubectl get all 
kubectl get pods -A
kubectl config current-context
```

## Kubeconfig

```shell
kind get kubeconfig >> ~/.kube/config
```

## Documentation

https://kind.sigs.k8s.io/docs/user/quick-start/
