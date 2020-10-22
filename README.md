# vSphere CPI+CSI Helm chart

Integrated CPI + CSI helm chart, focused on Rancher Catalog deployment, minimal config, maximum speed. Many thanks to the [vSphere CPI helm chart](https://github.com/helm/charts/tree/master/stable/vsphere-cpi) on which this chart is based. 

The [vSphere Cloud Provider Interface](https://github.com/kubernetes/cloud-provider-vsphere) handles cloud specific functionality for VMware vSphere infrastructure running on Kubernetes, the [vSphere Container Storage Interface (CSI)](https://cloud-provider-vsphere.sigs.k8s.io/concepts/csi_overview.html) is a specification designed to enable persistent storage volume management on Container Orchestrators (COs) like Kubernetes.

## Introduction

This chart deploys all components required to run the external vSphere CPI and also contains the CSI components required for cloud native storage.

Deployment using [Rancher's catalog apps](https://rancher.com/docs/rancher/v2.x/en/helm-charts/legacy-catalogs/) feature was the original focus of this project. This means it's also Helm compatible. 

More info on the motivation behind creating this integrated chart can be found in this article: https://medium.com/@stefanvangastel/moving-to-out-of-tree-kubernetes-vsphere-cpi-csi-in-seconds-fc0c494bb114

## Prerequisites

- vSphere 6.7U3+ (CSI v1.0.2) or vSphere 7.0+ (CSI v2.0.0)
- Kubernetes cluster version 1.14+ (CSI v1.0.2) or 1.16+ (CSI v2.0.0)
- VM's with harware version 15+ and vmtools installed on all nodes
- The ubuntu guest OS is recommended
- Manual steps are described in [this description](charts/vsphere-cpi-csi/v2.0.0/README.md) (Rancher) of in the [Kubernetes documentation](https://kubernetes.io/docs/tasks/administer-cluster/running-cloud-controller/#running-cloud-controller-manager) (Helm)

## Installing the Chart using Rancher catalog

1. Add this repo (https://github.com/stefanvangastel/vsphere-cpi-csi-helm.git) as a Helm 3 catalog.
1. Launch the app and follow the instructions

## Installing the Chart using Helm 3.0+

1. Clone this repository: 
   ```bash
   $ git clone https://github.com/stefanvangastel/vsphere-cpi-csi-helm.git
   ```
1. Enter the directory: 
   ```bash
   $ cd vpshere-cpi-csi-helm
   ```
1. Install the chart of choice (CSI v1.0.2 of v2.0.0), the vcenter config options are required at minimum: 
   ```bash
   $ helm install vsphere-cpi-csi \
        --namespace kube-system \
        ./charts/vsphere-cpi-csi/v2.0.0 \
        --set vcenter.host=vsphere.example.com \
        --set vcenter.username=johndoe \
        --set vcenter.password=s3cret \
        --set vcenter.datacenter=dc1
   ```

## Uninstalling the Chart

To uninstall/delete the `vsphere-cpi-csi` deployment:

```bash
$ helm delete vsphere-cpi-csi --namespace kube-system 
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the vSphere CPI+CSIchart and their default values.

> **Warning**: In creating this chart we aimed to make it as simple and fast to deploy as possible. Therefore you will not find an excess of options and configuration values.

|             Parameter                    |            Description              |                  Default               |
|------------------------------------------|-------------------------------------|----------------------------------------|
| `images.vsphereCloudControllerManager`   | Overwrite images .e.g from private | gcr.io/cloud-provider-vsphere/cpi/release/manager:v1.1.0 |
| `images.csiAttacher`                     | registry | quay.io/k8scsi/csi-attacher:v2.0.0 |
| `images.csiDriver`                       |                                     | gcr.io/cloud-provider-vsphere/csi/release/driver:v2.0.0 |
| `images.livenessProbe`                   |                                     | quay.io/k8scsi/livenessprobe:v1.1.0 |
| `images.vsphereSyncer`                   |                                     | gcr.io/cloud-provider-vsphere/csi/release/syncer:v2.0.0 |
| `images.csiProvisioner`                  |                                     | quay.io/k8scsi/csi-provisioner:v1.4.0 |
| `images.csiResizer`                      |                                     | quay.io/k8scsi/csi-resizer:v0.3.0 |
| `images.nodeDriverRegistrar`             |                                     | quay.io/k8scsi/csi-node-driver-registrar:v1.2.0 |
| `vcenter.host`                           | The vCenter host or ip              |             |         
| `vcenter.insecurehost`                   | Use insecure connection             | true |
| `vcenter.username: admin `               | vCenter username                    | |          
| `vcenter.password: root`                 | vCenter password                    | |        
| `vcenter.datacenter: DC1`                | Datacenter name                     | |
| `storageclass.name`                      | Name for the auto-created storageClass | vsphere-csi |
| `storageclass.default`                   | Make it the default storageClass | true |
| `storageclass.fstype`                    | Filesystem type to use e.g. `ext4` or `file` (v2.0.0 only) | ext4 |
| `storageclass.storagepolicyname`         | Storagepolicy name to use if given | my-storage-policy |
| `storageclass.datastoreurl`              | Optional datastore url     | |

> **Tip**: In addition all settings used in the [vSphere CPI helm chart](https://github.com/helm/charts/tree/master/stable/vsphere-cpi) are also useable in this chart.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install` using Helm v3.X. For example:

```bash
$ helm install vsphere-cpi-csi \
    --namespace kube-system \
    ./charts/vsphere-cpi-csi/v2.0.0 \
    --set vcenter.host=vsphere.example.com \
    --set vcenter.username=johndoe \
    --set vcenter.password=s3cret \
    --set vcenter.datacenter=dc1
```

Alternatively, a YAML file (e.g. `myvalues.yml`) that specifies the values for the parameters can be provided while installing the chart:

```bash
$ helm install vsphere-cpi-csi \
    ./charts/vsphere-cpi-csi/v2.0.0 \
    -f myvalues.yml
```