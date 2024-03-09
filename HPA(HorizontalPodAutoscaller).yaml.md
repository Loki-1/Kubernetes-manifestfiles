### basic deployment manifest file 
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: test-deployment-recreate
  namespace: test-ns
  labels:
    name: hpadeployment
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
          requests:
            memory: "128Mi"
            cpu: "500m"
          limits:
            memory: "256Mi"
            cpu: "500m"
        ports:
        - name: http
          containerPort: 8761

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
## basic HPA manifest file - manifest file start here
```
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: hpa
  namespace: test-ns
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: test-deployment-recreate
  minReplicas: 2
  maxReplicas: 5
  targetCPUUtilizationPercentage: 50

 ```     

### Practical Images:-

![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/ed991474-35fd-41ed-a461-c8bc484074d3)
![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/f10f1267-030f-4f1c-a1be-572c37dd005b)
![image](https://github.com/Loki-1/Kubernetes-manifestfiles/assets/134843197/018abfa1-81f9-42ae-8114-d0334cc075e5)

