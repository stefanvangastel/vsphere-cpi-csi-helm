## vSphere CPI v1.1.0 and CSI v1.0.2

This chart deploys both the vSphere CPI (Cloud Provider Interface) manager for vSphere as well as the CSI (Cloud Storage Interface) drivers and optional storageClass.

It uses [upstream VMWare vSphere manifests](https://github.com/kubernetes-sigs/vsphere-csi-driver/tree/master/manifests) with small tweaks to support Rancher RKE clusters.  with small tweaks to support Rancher RKE clusters. 

**Pre-requirements:**

* **vSphere 6.7U3+** 
* Kubernetes cluster version 1.14+
* VM's with harware version 15+ and `vmtools` installed on all nodes
* The `ubuntu` guest OS is recommended 
* **Manual** steps are described in the `Detailed Description` below. 
