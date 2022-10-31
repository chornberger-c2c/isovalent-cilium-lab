## CLI
```
export HUBBLE_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/hubble/master/stable.txt)
curl -L --remote-name-all https://github.com/cilium/hubble/releases/download/$HUBBLE_VERSION/hubble-linux-amd64.tar.gz\{,.sha256sum\}
sha256sum --check hubble-linux-amd64.tar.gz.sha256sum
tar xzvfC hubble-linux-amd64.tar.gz ~/bin/
```

## Setup 
```
cilium hubble enable --ui --wait=false
cilium hubble port-forward
hubble status
cilium hubble ui
```

## Commands
status
```
hubble status
hubble list nodes
```
observe
```
hubble observe –namespace backend –protocol dns
hubble observe –namespace frontend –http-path jobs
```
flows output as json
```
hubble observe -o jsonpb --last 1000 --follow | tee -a flows.json
hubble observe -o jsonpb --last 1000 > flows.json
```

## Documentation

https://docs.cilium.io/en/v1.12/gettingstarted/hubble/
