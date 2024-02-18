apiVersion: v1
kind: ReplicationController
metadata:
 name: test-rc
 namespace: test-ns
spec:
  replicas: 2
  selector:
    app: test-rc-app
  template:
   metadata:
    name: webapp1
    labels:
     app: test-rc-app
    namespace: test-ns
   spec:
    containers:
    - name: java-test
      image: lokeshnagam121/javaproject
      ports:
      - containerPort: 8761

---

apiVersion: v1
kind: Service
metadata:
 name: rc-svc
 namespace: test-ns
spec:
  type: NodePort
  selector:
   app: test-rc-app
  ports:
  - port: 80
    targetPort: 8761
    nodePort: 30001

 # The range of NodePort in Kubernetes Cluster must be in 30000 - 32767
