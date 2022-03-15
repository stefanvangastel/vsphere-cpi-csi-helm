## Manual steps, existing cluster
1. Merge the config below with your existing cluster config `kubelet` configuration (`Edit cluster -> Edit as YAML (Cluster options)`):
  ```
  kubelet:
    extra_args:
      cloud-provider: external
    extra_binds:
      - '/csi:/csi:rshared'
      - '/var/lib/csi/sockets/pluginproxy/csi.vsphere.vmware.com:/var/lib/csi/sockets/pluginproxy/csi.vsphere.vmware.com:rshared'
  ```
2. Worker nodes **need** to have a `node.cloudprovider.kubernetes.io/uninitialized=true:NoSchedule` taint, this is only automatically applied to *new* nodes. Apply this taint manually to **existing** nodes.
3. Deploy the chart in the **`kube-system`** namespace. 

## Manual steps, new cluster
1. When creating a new cluster, select the `External` cloudprovider. This way all deployed nodes will get the required taints.
2. Merge the config below with the generated cluster config `kubelet` configuration (`Edit as YAML (Cluster options)`):
  ```
  kubelet:
    extra_binds:
      - '/csi:/csi:rshared'
      - '/var/lib/csi/sockets/pluginproxy/csi.vsphere.vmware.com:/var/lib/csi/sockets/pluginproxy/csi.vsphere.vmware.com:rshared'
  ```
3. Create you cluster.
4. Deploy the chart in the **`kube-system`** namespace. 
