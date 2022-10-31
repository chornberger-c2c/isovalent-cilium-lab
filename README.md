# Quickstart to Cilium
1. [About](#about)
2. [What problem does Cilium solve](#what-problem-does-cilium-solve)
3. [Lab Quickstart](#lab-quickstart)
4. [Reference](#reference)

## About

The purpose of this lab is to get you acquainted with [Cilium](https://cilium.io/get-started/), [Hubble](https://github.com/cilium/hubble), the [Network Policy Editor](https://editor.cilium.io/) and a typical workflow on a locally provisioned Kubernetes cluster.

## What problem does Cilium solve?

Cilium can load code into the Kernel via [eBPF](https://ebpf.io/) (extended Berkeley Packet Filter).

The host Kernel - shared by all containers running on the same system - can now be programmed.

This allows for observability and security features without sidecar-containers.

Ephemeral Pods' IP addresses are not helpful - they are impossible to track going back in time - so here labels are used to identify traffic to and from endpoints.

## Lab Quickstart


1. Setup a local Kubernetes cluster with [kind](kind.md)

2. Install [cilium](cilium.md)

3. Setup [hubble](hubble.md)

4. Apply [network policies](network-policies.md)

5. Look at [observability](workflow-visualize-adapt.md)

6. Setup [cluster mesh](cluster-mesh.md)

7. Do the full exercise in the [lab](lab.md)

## Reference

### Getting started

For a first introduction, head over to this place.

https://cilium.io/get-started/

### Cilium labs by Isovalent

Here you can have a hands-on experience with a set of featured labs, including topics like "Introduction to eBPF" as well as BGP, Tetragon, IPSec and WireGuard.

https://isovalent.com/labs/

### Network policy editor

To edit your network policies interactively visit:

https://editor.cilium.io

### Operator for Red Hat OpenShift

You can also install Cilium as well as Isovalent Cilium Enterprise into an OpenShift Container Platform by using the operator.

https://catalog.redhat.com/software/operators/search?q=cilium

