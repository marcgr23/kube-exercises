EXERCISE 4
==========

Deployment recreate:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  strategy: 
    type: Recreate
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
        version: v1.0.0
    spec:
      containers:
      - name: nginx
        image: nginx:1.19.4
        resources:
          limits:
              memory: "256Mi"
              cpu: 100m
          requests:
              memory: "256Mi"
              cpu: 100m
        ports:
        - containerPort: 80
```
Deployment rollout deployment:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  strategy: 
    type: RollingUpdate
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
        version: v2.0.0
    spec:
      containers:
      - name: nginx
        image: nginx:1.19.4
        resources:
          limits:
              memory: "256Mi"
              cpu: 100m
          requests:
              memory: "256Mi"
              cpu: 100m
        ports:
        - containerPort: 80
````
Historial de versiones:
```
kubectl rollout history deployment nginx-deployment
```
Rollout:
```
kubectl rollout undo deployment nginx-deployment --to-revision=1
```