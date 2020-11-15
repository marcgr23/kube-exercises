EXERCISE 1
==========

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


kubectl apply -f kube.yml

kubectl logs -f nginx-exercise --tail=10

kubectl get pod -o wide

kubectl exec -it nginx-exercise -- sh

kubectl exec --stdin --tty nginx-exercise -- /bin/bash
cd /usr/share/nginx/html
cat index.html

kubectl get pod nginx-exercise --output=yaml