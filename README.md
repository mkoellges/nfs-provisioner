# Install a new nfs provisioner

Install a nf2 provisioner to enable nfs storage pvcs in kubernetes cluster.

If you want to change values of installation chart, use the inspect command to create the values file:

```sh
helm inspect values stable/nfs-client-provisioner > nfs-provisioner.values
```

```sh
helm install nfs-provisioner  --set nfs.server=nfs-server.example.com --set nfs.path=/srv/nfs/kubedata stable/nfs-client-provisioner --namespace=nfs-provisioner --create-namespace

# or if you want to install helm with changed values

helm install nfs-provisioner  ---values ./nfs-privisioner.values --set nfs.server=nfs-server.example.com --set nfs.path=/srv/nfs/kubedata stable/nfs-client-provisioner --namespace=nfs-provisioner --create-namespace

```

After the installation, check your nfs installation using

```sh
kubectl get all,ing,configmap,pv,pvc --all-namespaces --selector=release=nfs-provisioner -o wide
```


additional Parameters to be set:

| Parameter                           | Description                                                 | Default                                           |
| ----------------------------------- | ----------------------------------------------------------- | ------------------------------------------------- |
| `replicaCount`                      | Number of provisioner instances to deployed                 | `1`                                               |
| `strategyType`                      | Specifies the strategy used to replace old Pods by new ones | `Recreate`                                        |
| `image.repository`                  | Provisioner image                                           | `quay.io/external_storage/nfs-client-provisioner` |
| `image.tag`                         | Version of provisioner image                                | `v3.1.0-k8s1.11`                                  |
| `image.pullPolicy`                  | Image pull policy                                           | `IfNotPresent`                                    |
| `storageClass.name`                 | Name of the storageClass                                    | `nfs-client`                                      |
| `storageClass.defaultClass`         | Set as the default StorageClass                             | `false`                                           |
| `storageClass.allowVolumeExpansion` | Allow expanding the volume                                  | `true`                                            |
| `storageClass.reclaimPolicy`        | Method used to reclaim an obsoleted volume                  | `Delete`                                          |
| `storageClass.provisionerName`      | Name of the provisionerName                                 | null                                              |
| `storageClass.archiveOnDelete`      | Archive pvc when deleting                                   | `true`                                            |
| `storageClass.accessModes`          | Set access mode for PV                                      | `ReadWriteOnce`                                   |
| `nfs.server`                        | Hostname of the NFS server                                  | null (ip or hostname)                             |
| `nfs.path`                          | Basepath of the mount point to be used                      | `/ifs/kubernetes`                                 |
| `nfs.mountOptions`                  | Mount options (e.g. 'nfsvers=3')                            | null                                              |
| `resources`                         | Resources required (e.g. CPU, memory)                       | `{}`                                              |
| `rbac.create`                       | Use Role-based Access Control                               | `true`                                            |
| `podSecurityPolicy.enabled`         | Create & use Pod Security Policy resources                  | `false`                                           |
| `priorityClassName`                 | Set pod priorityClassName                                   | null                                              |
| `serviceAccount.create`             | Should we create a ServiceAccount                           | `true`                                            |
| `serviceAccount.name`               | Name of the ServiceAccount to use                           | null                                              |
| `nodeSelector`                      | Node labels for pod assignment                              | `{}`                                              |
| `affinity`                          | Affinity settings                                           | `{}`                                              |
| `tolerations`                       | List of node taints to tolerate                             | `[]`                                              |
