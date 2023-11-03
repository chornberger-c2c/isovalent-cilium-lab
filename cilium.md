# Cilium installation

## CLI Tool

```shell
curl -L --remote-name-all https://github.com/cilium/cilium-cli/releases/latest/download/cilium-linux-amd64.tar.gz\{,.sha256sum\}
sha256sum --check cilium-linux-amd64.tar.gz.sha256sum
tar xfz cilium-linux-amd64.tar.gz
mv cilium ~/bin
```

## Setup

```shell
cilium version
cilium install
cilium status
kubectl get cm cilium-config -o yaml -n kube-system
kubectl get all -n kube-system
cilium connectivity test
```

## Commands

Get cilium endpoints

```shell
kubectl get cep -A 
```

Get cilium network policies

```shell
kubectl get ciliumnetworkpolicy.cilium.io -A
```

## Documentation

https://docs.cilium.io/en/v1.12/
