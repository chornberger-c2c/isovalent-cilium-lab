# Workflow to visualize and adapt rules

## Concept of zero-trust

```
cilium connectivity test
BACKEND=$(kubectl get pods -n cilium-test -o jsonpath='{.items[0].metadata.name}')
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://cilium.io | head -1
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -Ik --connect-timeout 5 https://kubernetes.io | head -1
hubble observe -o jsonpb --last 1000 > flows.json
```

## Use flows in editor’s WebUI

Upload flows.json to https://editor.cilium.io

=> “Add rule” 

=> set a policy name (editor icon in the middle)

=> Download cilium network policy

## Now there is zero-trust by default

Apply locally

```
kubectl apply -f policy.yaml
```
Try access

```
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -L https://golem.de
```

=> won’t work 

## Create flows

```
hubble observe -o jsonpb --last 1000 > flows-2.json
```

## Use network policy editor

Upload flows-2.json to https://editor.cilium.io

=> “Add rule” 

=> set a policy name (editor icon in the middle)

=> Download cilium network policy

## Create kubernetes object from policy

backend-golem.de.yaml
```
apiVersion: cilium.io/v2
kind: CiliumNetworkPolicy
metadata:
  name: backend-golem.de
  namespace: default
spec:
  endpointSelector:
    matchLabels:
      app: backend
  ingress:
    - fromEndpoints:
        - matchLabels:
            app: frontend
  egress:
    - toFQDNs:
        - matchName: golem.de
      toPorts:
        - ports:
            - port: "443"
```

Apply locally
```
kubectl create -f backend-golem.de.yaml
```

Verify changes

```
kubectl -n cilium-test exec -ti ${BACKEND} -- curl -L https://golem.de
```
=> works!
