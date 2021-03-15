# Install a new nfs provisioner

Install a nf2 provisioner to enable nfs storage pvcs in kubernetes cluster.

If you want to change values of installation chart, use the inspect command to create the values file:

Create a values.yaml file with the following content:

```yaml
fullnameOverride: nfs-client-provisioner

nodeSelector:
  kubernetes.io/arch: arm64

strategyType: Recreate

storageClass:
  name: nfs-storage
  defaultClass: true
  archiveOnDelete: true
  allowVolumeExpansion: true
  provisionerName: storage.provisioner/nfs

nfs:
  server: nfs-server.example.com
  path: /srv/nfs/kubedata
```

```sh
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/

helm install nfs nfs-subdir-external-provisioner/nfs-subdir-external-provisioner -f values.yaml -n nfs-provisioner
```

After the installation, check your nfs installation using

```sh
kubectl get all,ing,configmap,pv,pvc --all-namespaces --selector=release=nfs-provisioner -o wide
```
