## I am using Database ReplicaSet for emptyDir volume example

### In Kubernetes, an EmptyDir volume is a type of volume that is created when a Pod is assigned to a Node and exists for the Pod’s lifetime. It's initially empty and is first created when a Pod is assigned to a Node, and exists for as long as that Pod is running on that Node.

### EmptyDir volumes are useful when a container needs temporary storage that's shared between containers in the same Pod. For example, you might use an EmptyDir volume to share data between containers in a Pod during the lifetime of a job.

### The contents of an EmptyDir volume persist across container restarts within the same Pod but are lost when the Pod is deleted or rescheduled to another Node.

## Here's a basic example of using an EmptyDir volume in a Pod manifest:
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
        - name: emptydir-volume
          mountPath: /data
      volumes:
      - name: emptydir-volume
        emptyDir: {}
---
