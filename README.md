# isovalent-cilium-lab

## What problem does Cilium solve?

Cilium can load code into the Kernel via eBPF (extended Berekely Packet Filter).

The host Kernel - shared by all containers running on the same system - can now be programmed.

This allows for observability and security features without sidecar-containers.

Ephemeral Pods' IP addresses are not helpful - they are impossible to track going back in time - so here labels are used to identify endpoints.

## Reference

### Getting started

For a first introduction, head over to this place.

https://cilium.io/get-started/

### Cilium labs by Isovalent

Here you can have a hands-on experience with a set of featured labs, including topics like "Introduction to eBPF" as well as BGP, Tetragon, IPSec and WireGuard.

https://isovalent.com/labs/

### Network policy editor

To edit your network policies interactively, go to

https://editor.cilium.io

### Operator for Red Hat OpenShift

You can also install Cilium into OpenShift Container Platform by using the operator

https://catalog.redhat.com/software/operators/search?q=cilium

## Lab Quickstart

1. Setup a local Kubernetes cluster with [kind](kind.md)

2. Install [cilium](cilium.md)

3. Setup [hubble](hubble.md)

4. Apply [network policies](network-policies.md)

5. Look at [observability](workflow-visualize-adapt.md)

6. Setup [cluster mesh](cluster-mesh.md)
