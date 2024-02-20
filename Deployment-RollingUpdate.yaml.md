### Basic Rolling Update deployment manifest file

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-deployment-rolling
  namespace: test-ns
spec:
  replicas: 2
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  minReadySeconds: 30
  selector:
    matchLabels:
      app: test-app
  template:
    metadata:
      labels:
        app: test-app
    spec:
      containers:
      - name: java-app
        image: lokeshnagam121/javaproject
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 8761

---
```
### basic nodeport service manifest file
```
apiVersion: v1
kind: Service
metadata:
  name: testsvc
  namespace: test-ns
spec:
  type: NodePort
  selector:
    app: testapp
  ports:
  - port: 80
    targetPort: 8761
    nodePort: 30008
```

### Imperative command for deployment :
kubectl set image deployment test-deployment-rolling java-app=test-deployment-recreate:3 -n test-ns


### Practical Images
![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/653d024d-35f8-4b7b-ab73-5a9b1372ed29)
