## CLI
```
curl -L --remote-name-all https://github.com/cilium/cilium-cli/releases/latest/download/cilium-linux-amd64.tar.gz\{,.sha256sum\}
sha256sum --check cilium-linux-amd64.tar.gz.sha256sum
tar xzvfC cilium-linux-amd64.tar.gz ~/bin/
```

## Setup
```
cilium version
cilium install
cilium status
kubectl get cm cilium-config -o yaml -n kube-system
kubectl get all -n kube-system
cilium connectivity test
```

## Commands

get cilium endpoints
```
kubectl get cep -A 
```

## Documentation

https://docs.cilium.io/en/v1.12/
