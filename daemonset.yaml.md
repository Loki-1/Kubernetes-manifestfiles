apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: test-ds
  namespace: test-ns
spec:
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

![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/e49067e2-6000-4b3f-a496-c2bae653eee4)
