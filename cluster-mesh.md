# Cluster Mesh

Install cilium on two clusters

```shell
cilium install --context cluster-a --cluster-id 1 --cluster-name cluster-a
cilium install --context cluster-b --cluster-id 2 --cluster-name cluster-b
```

Enable clustermesh on two clusters

```shell
cilium clustermesh enable --context cluster-a --service-type NodePort
cilium clustermesh enable --context cluster-b --service-type NodePort
```

Connect both clusters

```shell
cilium clustermesh connect --context cluster-a --destination-context cluster-b
```

Make a service global

Use-case: fault-tolerance, shared services, multi cloud

```shell
kubectl --context cluster-a annotate service my-service io.cilium/global-service="true"
```

Disable a shared service

Use-case: service can no longer be reached from other cluster

```shell
kubectl --context cluster-b annotate service my-service io.cilium/shared-service="false"
```
