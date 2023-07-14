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

This allows for observability and security features without sidecar containers.

Ephemeral Pods' IP addresses are not helpful - they are impossible to track going back in time - so here labels are used to identify traffic to and from endpoints.

## Lab Quickstart

In this lab, we will setup some network policies and observe the network traffic going out of a Kubernetes cluster.

Please start [here](lab.md) directly.

## Reference

### Getting started

For a first introduction, head over to this place.

https://cilium.io/get-started/

### Cilium labs by Isovalent

Here you can have a hands-on experience with a set of featured labs, including topics like "Introduction to eBPF" as well as BGP, Tetragon, IPSec and WireGuard.

https://isovalent.com/resource-library/labs/

### Network policy editor

To edit your network policies interactively visit:

https://editor.cilium.io

### Operator for Red Hat OpenShift

You can also install Cilium into an OpenShift Container Platform by using the operator.

https://catalog.redhat.com/software/container-stacks/detail/60423ec2c00b1279ffe35a68
