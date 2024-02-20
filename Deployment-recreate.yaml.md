### basic deployment manifest file 
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-deployment-recreate
  namespace: test-ns
spec:
  replicas: 2
  strategy:
    type: Recreate
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
### basic NodePort Service manifest file
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

kubectl set image deployment test-deployment-recreate java-app=test-deployment-recreate:3 -n test-ns

# practical Images

![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/86768f73-9b4b-49c4-8240-772897c94397)
![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/ab5147b4-da36-4dcd-bc88-64fa3d70f477)

