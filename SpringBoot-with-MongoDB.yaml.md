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

![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/324c03f1-ddbc-4618-9267-40216e256e29)

![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/2894f219-572e-42dd-979e-f077e6566e4b)

