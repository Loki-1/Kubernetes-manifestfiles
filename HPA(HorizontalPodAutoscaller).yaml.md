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
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: hpadeploymentautoscaler
  namespace: test-ns
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: test-deployment-recreate
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
        name: cpu
        target:
        - type: Utilization
          averageUtilization: 50
  - type: Resource
    resource:
        name: memory
        target:
        - type: Utilization
          averageUtilization: 50
 ```     

### Practical Images:-
