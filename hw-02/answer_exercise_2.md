EXERCISE 2
==========

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-exercise2
  labels:
    app: nginx-server2
    tier: kube
spec:
  replicas: 3
  selector:
    matchLabels:
      tier: kube
  template:
    metadata:
      labels:
        tier: kube
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.19.4


kubectl scale rs nginx-exercise2 --replicas=10

Un DaemonSet garantiza que todos (o algunos) de los nodos ejecuten una copia de un Pod. Conforme se añade más nodos al clúster, nuevos Pods son añadidos a los mismos. Conforme se elimina nodos del clúster, dichos Pods se destruyen.