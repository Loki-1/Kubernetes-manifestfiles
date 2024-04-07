'''
apiVersion: apps/v2
kind: Deployment
metadata:
  name: springboot-app
  namespace: test-ns
spec:
  replicas: 2
  selector:
    matchLabels:
      app: SB-app
  template:
    metadata:
      labels:
        app: SB-app
    spec:
      containers:
      - name: sb-container
        image: lokeshnagam121/springboot
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "500Mi"
            cpu: "500m"
        ports:
        - containerPort: 8080
        env:
        - name: MONGO_DB_HOSTNAME
          value: SB-DB-SVC
        - name: MONGO_DB_USERNAME
          value: devdb
        - name: MONGO_DB_PASSWORD
          value: devdb@123
---
'''
