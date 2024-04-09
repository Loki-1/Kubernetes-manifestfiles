### In Kubernetes, an NFS (Network File System) volume is a type of volume that allows you to mount a directory from an NFS server into your Kubernetes pods. This enables your pods to read from and write to files stored on a remote NFS server. NFS volumes are useful for scenarios where you need to share data across multiple pods or nodes in a Kubernetes cluster.

### KeyPoints:

**External Storage:** NFS volumes provide a way to use external storage resources in your Kubernetes cluster. You can leverage existing NFS servers to store and manage your data, which can be particularly useful for sharing files between different applications or services running in the cluster.

**Networked Storage:** NFS volumes rely on the NFS protocol to access files over the network. This means that the NFS server hosting the data can be located on a different machine or network from the Kubernetes cluster itself. It also allows for centralized storage management and scalability.

**Data Persistence:** Data stored in NFS volumes persists independently of the lifecycle of individual pods or nodes. This means that even if a pod is deleted or rescheduled, the data stored in the NFS volume remains accessible to other pods or nodes that mount the same volume.


## Drawbacks:

**Single Point of Failure:** NFS servers can become a single point of failure. If the NFS server goes down, all clients lose access to the shared data until the server is restored. Implementing high availability configurations can mitigate this, but it adds complexity and cost.

**Performance Bottlenecks:** NFS performance can degrade under heavy load or when transferring large files due to its architecture. Network latency, server load, and disk I/O can all impact performance. Additionally, NFS might not be the best choice for highly I/O intensive workloads.

## Usage:

### Create a server with any configuration as you want for your volume

### enter below commands
```
sudo apt install net-tools
sudo netstat -tunlp      (nfs not installed yet need to install)
sudo apt-get update
sudo apt-get install nfs-kernel-server -y
sudo mkdir -p /mnt/nfs_share
sudo chmod 777 /mnt/nfs_share
sudo chown -R nobody:nogroup /mnt/nfs_share/
sudo apt-get install nfs-common -y
```
### Try to add a line in below file using Vi editor
sudo vi /etc/exports  (add below line at the end of file)
```
/mnt/nfs_share/   *(rw,sync,no_subtree_check,no_root_squash)
```
### Now enter below commands
```
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```
### Here's a basic example of using a NFS volume in a Kubernetes Pod manifest:
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
        resources:
          limits:
            memory: "500Mi"
            cpu: "500m"
        ports:
        - containerPort: 27017
        env:
        - name: MONGO-INITDB-ROOT-USERNAME
          value: devdb
        - name: MONGO-INITDB-ROOT-PASSWORD
          value: devdb@123
        volumeMounts:
        - name: nfs-volume
          mountPath: /data/db
      volumes:
      - name: nfs-volume
        nfs:
          server:<private_ip-of NFS server>
          path: /mnt/nfs_share/
---
```  
### enter below command on the nfs server to check k8s cluster pods data
```
ls /mnt/nfs_share/
```
