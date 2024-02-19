### ReplicaSet Expression Based selector manifest file
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: test-rs-2
  namespace: test-ns
spec:
  replicas: 2
  selector: # this selector data basically based on expression it will work for both test-app and test-app2 values
   matchExpressions:
   - key: app
     operator: In
     values:
     - "test-app"
     - "test-app2"
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
    nodePort: 30005
```
### Practical Image

![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/28e9bb13-70f6-48c7-879d-de94ad2217f9)
