# Cilium installation

## CLI Tool

```shell
curl -L --remote-name-all https://github.com/cilium/cilium-cli/releases/latest/download/cilium-linux-amd64.tar.gz\{,.sha256sum\}
sha256sum --check cilium-linux-amd64.tar.gz.sha256sum
tar xzvfC cilium-linux-amd64.tar.gz ~/bin/
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

- get cilium endpoints

```shell
kubectl get cep -A 
```

- get cilium network policies

```shell
kubectl get ciliumnetworkpolicy.cilium.io -A
```

## Documentation

https://docs.cilium.io/en/v1.12/
