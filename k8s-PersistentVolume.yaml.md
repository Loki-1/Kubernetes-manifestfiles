![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/65528f53-f8ab-4671-808e-11adca75d03a)
## Persistent Volumes (PVs):

**Think of PVs as virtual disks ( Storage resources) that are available for use within the Kubernetes cluster.
Administrators set up PVs by defining the storage type (e.g., NFS, iSCSI), size, access modes, and other parameters in YAML files.**
PVs represent the actual physical or cloud-based storage(EBS,Azuredisk,Azurefile,gce e.t.c) that is provisioned and managed by the cluster administrator.
They exist independently of any pods that use them.
These are Independent of the NameSpaces.
One PV can connect with one PVC at a time. 
We can create PV's in 2 ways
1.Static Volumes 
2.Dynamic Volumes

### 1. Static Persistent Volume: 

Static PVs are provisioned manually by the cluster administrator.
Administrators create PV objects in advance, specifying details such as storage type, size, access modes, and other parameters.
These PV objects represent pre-allocated storage resources within the cluster.
Static PVs are typically used when the storage infrastructure is already set up and available before applications are deployed.
Pods can then claim these pre-existing PVs by referencing them in their Persistent Volume Claims (PVCs).

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-nfs-pv
spec:
  capacity:
    storage: 2Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  storageClassName: nfs
  nfs:
    server: <server-ip/domain>
    path: /path/to/nfs/share
```
### 2. Dynamic Persistent Volumes (PVs):

Dynamic PVs are provisioned automatically by Kubernetes in response to Persistent Volume Claims (PVCs).
When a PVC is created, Kubernetes matches it with a suitable storage class that defines how to provision storage dynamically.
The storage class specifies the parameters for creating PVs dynamically, such as the type of storage, size, access modes, and any other required configurations.
If the PVC requests storage that matches the criteria defined in the storage class and there are no available static PVs that satisfy the claim, Kubernetes automatically provisions a new PV.
Dynamic provisioning simplifies storage management by allowing Kubernetes to create PVs on-demand as needed by applications, without manual intervention from administrators.

## Persistent Volume Claims (PVCs):

**PVCs are like requests made by applications (or developers) for storage resources.
Developers define PVCs in YAML files, specifying the storage requirements such as size, access mode, and storage class.**
When a pod needs storage, it references a PVC rather than directly interacting with a PV.
Kubernetes dynamically binds PVCs to PVs that match the requested criteria.
If a suitable PV isn't available, Kubernetes can dynamically provision one based on the PVC specifications based on storage class.
One PVC can connect with one PV at a time. 
NameSpace Specific

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: mongo-db
  namespace: test-ns
spec:
  selector:
    matchLabels:
      app: myDB
  template:
    metadata:
      name: mongo-db
      labels:
        app: myDB
    spec:
      containers:
      - name: mongo-db
        image: mongo
        ports:
        - containerPort: 27017
        env:
        - name: MONGO-INITDB-ROOT-USERNAME
          value: devdb
        - name: MONGO-INITDB-ROOT-PASSWORD
          value: devdb@123
        volumeMounts:
        - name: mongo-db
          mountPath: /data/db
      volumes:
      - name: mongo-db
        PersistentVolumeClaim:
         claimName: my-pvc
---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```



### Binding of PVCs and PVs:

When a PVC is created, Kubernetes searches for a PV that satisfies the PVC's requirements.
If a suitable PV is found, the PVC is bound to that PV, and the pod can use the storage.
If no matching PV exists, and the storage class allows dynamic provisioning, Kubernetes automatically provisions a new PV that matches the PVC's requirements.

### Usage in Pods:

Pods consume storage through PVCs.
Inside the pod's YAML definition, developers specify the PVC they want the pod to use.
Kubernetes ensures that the pod has access to the storage provided by the bound PV.

### Storage Lifecycle Management:

Administrators manage the lifecycle of PVs, including provisioning, resizing, and deletion.
Developers manage PVCs, creating, deleting, and modifying them as needed for their applications.

#### Kubernetes Commands:-
```
Kubectl get pv
Kubectl get pvc
kubectl get storageclass
```
