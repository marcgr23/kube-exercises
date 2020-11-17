EXERCISE 5
==========

Deployment v1:
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
        version: v1.1.0
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
Deployment v2:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment2
  labels:
    app: nginx2
spec:
  replicas: 3
  strategy: 
    type: Recreate
  selector:
    matchLabels:
      app: nginx2
  template:
    metadata:
      labels:
        app: nginx2
        version: v1.2.0
    spec:
      containers:
      - name: nginx2
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
El servicio apunta al deployment v1:
```
apiVersion: v1
kind: Service
metadata:
  name: service1
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
```
El mismo servicio pasa a apuntar al deployment v2:
```
apiVersion: v1
kind: Service
metadata:
  name: service1
spec:
  type: NodePort
  selector:
    app: nginx2
  ports:
    - port: 80
      targetPort: 80
```
Comprobamos donde apunta el servicio:
```
kubectl get pods -o wide
kubectl describe service service1
```