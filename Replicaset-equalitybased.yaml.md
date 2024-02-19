### Replicaset Manifest File
```
apiVersion: apps/v1
kind: ReplicationSet
metadata:
  name: test-rs
  namespace: test-ns
spec:
  replicas: 2
  selector:  # Mandatory and below selector is equality based selector
   matchLabels:
   - app: test-app
  template: #pod template
    metadata:
      name: javaapp
      labels:
        app: test-app
    spec:
      containers:
      - name: javacontainer
        image: lokeshnagam121/javaproject
        ports:
        - containerPort: 8761

---
```

### Nodeport Service manifest file 
```
apiVersion: v1
kind: Service
metadata:
 name: rs-svc
 namespace: test-ns
spec:
  type: NodePort
  selector:
   app: test-app
  ports:
  - port: 80
    targetPort: 8761
    nodePort: 30759
```
### practical Image
![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/feca187d-68f9-411e-b5fb-4b4ab3b88ec9)
