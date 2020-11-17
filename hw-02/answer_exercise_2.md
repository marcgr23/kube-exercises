EXERCISE 2
==========

Objeto ReplicaSet:
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-exercise2
  labels:
    app: nginx-server2
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-server2
  template:
    metadata:
      labels:
        app: nginx-server2
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.19.4
```
Escalar a 10 replicas:
```
kubectl scale rs nginx-exercise2 --replicas=10
````

El objeto que mejor se adaptaria es un DaemonSet, porque garantiza que todos (o algunos) de los nodos ejecuten una copia de un Pod. Conforme se añade más nodos al clúster, nuevos Pods son añadidos a los mismos. Y de la misma forma conforme se eliminan nodos del clúster, dichos Pods se destruyen.