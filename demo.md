# Demo for IT Tage 2023

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
