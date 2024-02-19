
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: test-rs-2
  namespace: test-ns
spec:
  replicas: 2
  selector:
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

