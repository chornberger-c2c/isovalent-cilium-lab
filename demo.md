# Demo Session

## Prerequisites for local cluster with kind

### Inotify Settings for kind

See https://kind.sigs.k8s.io/docs/user/known-issues/#pod-errors-due-to-too-many-open-files

```shell
sudo sysctl fs.inotify.max_user_watches=524288
sudo sysctl fs.inotify.max_user_instances=512
```

### Create a cluster with kind

```shell
curl -Lo cluster.yml https://raw.githubusercontent.com/chornberger-c2c/isovalent-cilium-lab/main/kind/cluster.yml
kind create cluster --name cilium --config cluster.yml
```

## Prerequisites for Cilium and Hubble

### Setup Cilium and Hubble

```shell
cilium install  
cilium hubble enable --ui
cilium hubble port-forward &
cilium hubble ui &
```

### Cilium connectivity tests

```shell
cilium connectivity test
```

## Observing and allowing Traffic

```shell
# Set BACKEND to a pod name
BACKEND=$(kubectl get pods -n cilium-test -o jsonpath='{.items[0].metadata.name}')

# Generate traffic to insecure.org
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://reverse-shell.insecure.org

# Observe traffic to insecure.org
hubble observe  --from-pod cilium-test/"$BACKEND"

# Egress default deny
kubectl apply -f https://raw.githubusercontent.com/chornberger-c2c/isovalent-cilium-lab/main/cilium-network-policies/egress-default-deny.yml

# Generate traffic to insecure.org
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://reverse-shell.insecure.org

# Observe traffic to insecure.org
hubble observe  --from-pod cilium-test/"$BACKEND"

# Generate traffic to cilium.io
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://cilium.io

# Observe traffic to cilium.io
hubble observe  --from-pod cilium-test/"$BACKEND"

# Allow cilium.io
kubectl apply -f https://raw.githubusercontent.com/chornberger-c2c/isovalent-cilium-lab/main/cilium-network-policies/allow-cilium-io.yml

# Generate traffic to cilium.io
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://cilium.io

# Observe traffic to cilium.io
hubble observe  --from-pod cilium-test/"$BACKEND"
```

## Star Wars Demo

### Setup deployments, pods, services

```shell
kubectl create -f https://raw.githubusercontent.com/cilium/cilium/1.14.4/examples/minikube/http-sw-app.yaml
```

### Check Access

```shell
kubectl exec xwing -- curl -s -XPOST deathstar.default.svc.cluster.local/v1/request-landing
kubectl exec tiefighter -- curl -s -XPOST deathstar.default.svc.cluster.local/v1/request-landing
```

### Apply L3/L4 Policy

```shell
kubectl create -f https://raw.githubusercontent.com/cilium/cilium/1.14.4/examples/minikube/sw_l3_l4_policy.yaml
```

### Check Access

```shell
kubectl exec tiefighter -- curl -s -XPOST deathstar.default.svc.cluster.local/v1/request-landing
kubectl exec xwing -- curl -s -XPOST deathstar.default.svc.cluster.local/v1/request-landing
```

### Check L7-aware Policy

```shell
kubectl exec tiefighter -- curl -s -XPUT deathstar.default.svc.cluster.local/v1/exhaust-port
```

### Apply L7-aware Policy

```shell
kubectl apply -f https://raw.githubusercontent.com/cilium/cilium/1.14.4/examples/minikube/sw_l3_l4_l7_policy.yaml
```

### Check Access

```shell
kubectl exec tiefighter -- curl -s -XPOST deathstar.default.svc.cluster.local/v1/request-landing
kubectl exec tiefighter -- curl -s -XPUT deathstar.default.svc.cluster.local/v1/exhaust-port
kubectl exec xwing -- curl -s -XPOST deathstar.default.svc.cluster.local/v1/request-landing
```
