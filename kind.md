## CLI 
```
  curl -Lo ./kind "https://kind.sigs.k8s.io/dl/v0.14.0/kind-$(uname)-amd64"
  chmod +x kind
  mv kind ~/bin
```

## setup
``` 
 kind create cluster
 kubectl get all 
 kubectl get pods -A
 kubectl config current-context

```
## kubeconfig
```
kind get kubeconfig > ~/.kube/config
```
