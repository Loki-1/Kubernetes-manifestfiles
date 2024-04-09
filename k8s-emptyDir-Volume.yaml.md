## I am using Database ReplicaSet for emptyDir volume example
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
