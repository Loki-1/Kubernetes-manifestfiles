# pod with 2 containers
```
apiVersion: v1
kind: Pod
metadata:
  name: testpod
  namespace: test-ns
  labels:
    app: testapp
spec:
  containers:
  - name: test-container1
    image: nginx
    ports:
    - containerPort: 8080
  - name: test-container2
    image: lokeshnagam121/javaproject
    ports:
    - containerPort: 8761
    
---
```
# NodePort Service with 2 ports for 2 containers
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
  - port: 8080
    targetPort: 8080
    name: nginx
  - port: 8761
    targetPort: 8761
    name: javaapp
 ```   
![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/2b0e2934-0ac5-4ea0-9466-509a32dc6063)

