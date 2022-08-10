## Cluster Mesh

### install cilium on two clusters

```
cilium install --context cluster-a --cluster-id 1 --cluster-name cluster-a
cilium install --context cluster-b --cluster-id 2 --cluster-name cluster-b
```

### enable clustermesh on two clusters

```
cilium clustermesh enable --context cluster-a --service-type NodePort
cilium clustermesh enable --context cluster-b --service-type NodePort
```

### connect both clusters

```
cilium clustermesh connect --context cluster-a --destination-context cluster-b
```

### make a service global 

Use-case: fault-tolerance, shared services, multi cloud

```
kubectl --context cluster-a annotate service my-service io.cilium/global-service="true"
```

### disable a shared service

Use-case: service can no longer be reached from other cluster

```
kubectl --context cluster-b annotate service my-service io.cilium/shared-service="false"
```
