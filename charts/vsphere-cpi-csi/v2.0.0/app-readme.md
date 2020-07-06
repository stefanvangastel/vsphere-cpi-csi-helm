## vSphere CPI v1.1.0 and CSI v2.0.0

This chart deploys both the vSphere CPI (Cloud Provider Interface) manager for vSphere as well as the CSI (Cloud Storage Interface) drivers and optional storageClass.

It uses [upstream VMWare vSphere manifests](https://github.com/kubernetes-sigs/vsphere-csi-driver/tree/master/manifests) with small tweaks to support Rancher RKE clusters. 

**Pre-requirements:**

* **vSphere 7.0+** 
* Kubernetes cluster version **1.16+**
* VM's with harware version 15+ and `vmtools` installed on all nodes
* The `ubuntu` guest OS is recommended 
* **Manual** steps are described in the `Detailed Description` below. 
