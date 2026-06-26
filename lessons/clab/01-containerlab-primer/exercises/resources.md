1. https://containerlab.dev/manual/config-mgmt/#startup-configuration-file

You specify license under node 

```yaml
topology:
  nodes:
    srl1:
      license: license.key
    srl2:
      license: https://licenses.example.com/srl2/license.key
    srl3:
      license: sftp://user@sftp.example.com/licenses/srl3/license.key
    srl4:
      license: |
        embedded license content
```

2. https://github.com/aadhilam/k8-networking-calico-containerlab 

It demonstrates how to to setup networking with calico in kubernetes

3. Won't join discord at the moment
