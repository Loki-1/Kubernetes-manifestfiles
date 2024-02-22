````
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hpadeployment
  namespace: test-ns
  labels:
    name: hpadeployment
spec:
  replicas: 2
  selector:
    matchLabels:
      name: hpapod
  template:
    metadata:
      labels:
        name: hpapod
    spec:
      containers:
        - name: hpacontainer
          image: lokeshnagam121/javaproject
          ports:
          -  containerPort: 80
          resources:
            requests:
              cpu: "100m"
              memory: "64Mi"
            limits:
              cpu: "100m"
              memory: "256Mi"
---
```
apiVersion: v1
kind: Service
metadata:
  name: hpaclusterservice
  namespace: test-ns
  labels:
    name: hpaservice
spec:
  type: ClusterIP
  selector:
    name: hpapod
  ports:
    - port: 80
      targetPort: 80
```
````
