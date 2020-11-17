EXERCISE 1
==========

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-exercise
  labels:
    app: nginx-server
spec:
  containers:
  - name: nginx-container
    image: nginx:1.19.4
    resources:
        limits:
            memory: "256Mi"
            cpu: 100m
        requests:
            memory: "256Mi"
            cpu: 100m
```
Levantar Pod:
```
kubectl apply -f kube.yml
````
10 líneas de salida:
```
kubectl logs -f nginx-exercise --tail=10
```
Ip del Pod:
```
kubectl get pod -o wide
````
Entrar en el Pod:
```
kubectl exec -it nginx-exercise -- sh
kubectl exec --stdin --tty nginx-exercise -- /bin/bash
cd /usr/share/nginx/html
cat index.html
```
Calidad del servicio:
```
kubectl get pod nginx-exercise --output=yaml
```