## CLI
```
export HUBBLE_VERSION=$(curl -s https://raw.githubusercontent.com/cilium/hubble/master/stable.txt)
curl -L --remote-name-all https://github.com/cilium/hubble/releases/download/$HUBBLE_VERSION/hubble-linux-amd64.tar.gz\{,.sha256sum\}
sha256sum --check hubble-linux-amd64.tar.gz.sha256sum
tar xzvfC hubble-linux-amd64.tar.gz ~/bin/
```

## setup 
```
cilium hubble enable --ui --wait=false
cilium hubble port-forward&
hubble status
cilium hubble ui
```

## commands
```
hubble status
hubble list nodes
hubble observe –namespace backend –protocol dns
hubble observe –namespace frontend –http-path jobs
```
