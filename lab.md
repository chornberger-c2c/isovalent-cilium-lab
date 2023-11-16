# Lab

## Create a cluster with kind

### Prerequisites

If not done already, please follow the instructions to install [kind](kind.md).

### Outcome

You will have a local Kubernetes cluster with four nodes, running inside four containers.

```shell
curl -Lo cluster.yml https://raw.githubusercontent.com/chornberger-c2c/isovalent-cilium-lab/main/kind/cluster.yml
kind create cluster --name cilium --config cluster.yml
```

## Install and setup Cilium with Hubble

### Prerequisites

If not done already, please follow the instructions to get the [cilium](cilium.md) and [hubble](hubble.md) utilities.

### Outcome

Cilium and hubble will be installed on this local Kubernetes cluster.

```shell
cilium install  
cilium hubble enable --ui
cilium hubble port-forward &
cilium hubble ui &
cilium connectivity test
```

=> This creates namespace cilium-test which we will use later on!

## Verify that outgoing requests work

We test the connection from the first pod in namespace cilium-test to https://cilium.io and get a positive return code.

```shell
BACKEND=$(kubectl get pods -n cilium-test -o jsonpath='{.items[0].metadata.name}')
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://cilium.io | head -1
HTTP/2 200
```

## Apply a first policy for zero trust

```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: cilium-test
  namespace: cilium-test
spec:
  endpointSelector: {}
  egress:
    - toEndpoints:
        - matchLabels:
            io.kubernetes.pod.namespace: kube-system
            k8s-app: kube-dns
      toPorts:
        - ports:
            - port: "53"
              protocol: UDP
          rules:
            dns:
              - matchPattern: "*"
```

or go to https://editor.networkpolicy.io/ and do it manually

![create policy](pictures/editor-cilium-io-1.png)

* "Create new policy" (empty page bottom left)
* "Edit" icon in the middle of the page => enter a namespace and a policy name

![deny egress](pictures/editor-cilium-io-2.png)

* "Egress Default Deny"
* "Allow Kubernetes DNS"
* "Download"

Apply locally

```shell
kubectl apply -f https://raw.githubusercontent.com/chornberger-c2c/isovalent-cilium-lab/main/cilium-network-policies/egress-default-deny.yml
```

Observe the change

```shell
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://cilium.io | head -1
curl: (28) Connection timeout after 5001 ms
command terminated with exit code 28
hubble observe --output jsonpb --last 1000  > backend-cilium-io.json
```

=> The connection to https://cilium.io won't work, as we configured "Egress Default Deny" in our first policy.

## Apply new rule that allows access to cilium.io

```yaml
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: cilium-test
  namespace: cilium-test
spec:
  endpointSelector: {}
  egress:
    - toEndpoints:
        - matchLabels:
            io.kubernetes.pod.namespace: kube-system
            k8s-app: kube-dns
      toPorts:
        - ports:
            - port: "53"
              protocol: UDP
          rules:
            dns:
              - matchPattern: "*"
    - toFQDNs:
        - matchName: cilium.io
      toPorts:
        - ports:
            - port: "443"
```

or go to https://editor.cilium.io and do it manually

![upload flows and add rule](pictures/editor-cilium-io-3.png)

* "Flows upload"
* "Upload flows"
* "Add rule"
* "Download"

![rule added](pictures/editor-cilium-io-4.png)

Apply locally

```shell
kubectl apply -f https://raw.githubusercontent.com/chornberger-c2c/isovalent-cilium-lab/main/cilium-network-policies/allow-cilium-io.yml
```

## Verify

```shell
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://kubernetes.io | head -1
curl: (28) Connection timeout after 5001 ms
command terminated with exit code 28
```

=> Timeout indicates that the connection to https://kubernetes.io doesn't work, as of "Egress Default Deny".

```shell
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://cilium.io | head -1
HTTP/2 200
```

=> Positive return code shows that the connection to https://cilium.io works, as of our applied policy.
