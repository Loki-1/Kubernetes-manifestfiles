```
apiVersion: apps/v2
kind: Deployment
metadata:
  name: springboot-app
  namespace: test-ns
spec:
  replicas: 2
  selector:
    matchLabels:
      app: SB-app
  template:
    metadata:
      labels:
        app: SB-app
    spec:
      containers:
      - name: sb-container
        image: lokeshnagam121/springboot
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "500Mi"
            cpu: "500m"
        ports:
        - containerPort: 8080
        env:
        - name: MONGO_DB_HOSTNAME
          value: SB-DB-SVC
        - name: MONGO_DB_USERNAME
          value: devdb
        - name: MONGO_DB_PASSWORD
          value: devdb@123
---
```

```
apiVersion: v1
kind: Service
metadata:
  name: sb-svc
  namespace: test-ns
spec:
  type: NodePort
  selector:
    app: SB-app
  ports:
  - port: 80
    targetPort: 8080

---
```

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

---
```
apiVersion: v1
kind: Service
metadata:
  name: sb-db-svc
  namespace: test-ns
spec:
  type: ClusterIP
  selector:
    app: myDB
  ports:
  - port: 27017
    targetPort: 27017
```
