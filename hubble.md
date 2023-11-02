# Hubble Installation

## CLI Tool

```shell
export HUBBLE_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/hubble/master/stable.txt)
curl -L --remote-name-all https://github.com/cilium/hubble/releases/download/$HUBBLE_VERSION/hubble-linux-amd64.tar.gz\{,.sha256sum\}
sha256sum --check hubble-linux-amd64.tar.gz.sha256sum
tar xzvfC hubble-linux-amd64.tar.gz ~/bin/
```

## Setup

```shell
cilium hubble enable --ui --wait=false
cilium hubble port-forward
hubble status
cilium hubble ui
```

## Commands

Status

```shell
hubble status
hubble list nodes
```

Observe

```shell
hubble observe –namespace backend –protocol dns
hubble observe –namespace frontend –http-path jobs
```

Output flows as json

```shell
hubble observe -o jsonpb --last 1000 --follow | tee -a flows.json
hubble observe -o jsonpb --last 1000 > flows.json
```

## Documentation

https://docs.cilium.io/en/v1.12/gettingstarted/hubble/
