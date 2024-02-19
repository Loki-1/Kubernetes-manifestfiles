### Replicaset Manifest File

apiVersion: apps/v1
kind: ReplicationSet
metadata:
  name: test-rs
  namespace: test-ns
spec:
  replicas: 2
  selector:
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