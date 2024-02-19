### basic deamonset manifest file - it will create pod  per node - if we have 10 nodes then it will create 10 pods ( 1 per node)
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: test-ds
  namespace: test-ns
spec: # here we dont see replicas info like rc and rs because it will create a pod per node by default
  selector:
   matchLabels:
    app: test-app
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

### basic manifest file for NodePort Service
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
    nodePort: 30096
```
![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/e49067e2-6000-4b3f-a496-c2bae653eee4)
