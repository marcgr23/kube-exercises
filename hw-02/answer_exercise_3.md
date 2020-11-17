EXERCISE 3
==========

Servicio al exterior:
```
apiVersion: v1
kind: Service
metadata:
  name: service1
spec:
  type: NodePort
  selector:
    app: nginx-server2
  ports:
    - port: 80
      targetPort: 80
````
Ver si tiene endpoint activo:
```
kubectl describe service service1
```
Url para conectarse:
```
minikube service service1 --url
```
Servicio interno:
```
apiVersion: v1
kind: Service
metadata:
  name: service2
spec:
  selector:
    app: nginx-server2
  ports:
    - port: 80
      targetPort: 80
```
Servicio con puerto específico:
```
apiVersion: v1
kind: Service
metadata:
  name: service3
spec:
  type: NodePort
  selector:
    app: nginx-server2
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30765
```