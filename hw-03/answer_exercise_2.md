EXERCISE 2
==========

StatefulSet:
```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mongo
spec:
  selector:
    matchLabels:
      app: db
      name: mongodb
  serviceName: mongodb-svc
  replicas: 3
  template:
    metadata:
      labels:
        app: db
        name: mongodb
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: mongo
        image: mongo:4.4.1
        command:
          - mongod
        args:
          - --bind_ip=0.0.0.0
          - --replSet=rs0
          - --dbpath=/data/db
        ports:
          - containerPort: 27017
        volumeMounts:
          - name: mongo-persistent-storage
            mountPath: /data/db
  volumeClaimTemplates:
    - metadata:
        name: mongo-persistent-storage
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 20Gi
```

Para comprobar el funcionamiento del StatefulSet se ha creado un usuario en un nodo:
```
db.createUser(
   {
     user: "marc",
     pwd: "1234",
     roles: [ { role: "readWrite", db: "mongodb" } ]
   }
)
```

Para comprobar si el usuario se ha creado:
```
db.getUsers( {
   showCredentials: true,
   filter: {}
} )
```

Para convertir un nodo en el nodo primario, hay que ejecutar este comando en uno de los tres nodos:
```
rs.initiate(
   {
      _id: "rs0",
      version: 1,
      members: [
         { _id: 0, host : "172.17.0.3:27017" },
         { _id: 1, host : "172.17.0.4:27017" },
         { _id: 2, host : "172.17.0.5:27017" }
      ]
   }
)
```

Para comprobar que el usuario esta dado de alta en los otros nodos, hay que entrar en otro nodo y ejecutar:
```
mongo --username marc --pasword 1234 --authenticationDatabase mongodb
```

El ReplicaSet garantiza que un número específico de réplicas de un pod se está ejecutando en todo momento, pero no garantiza que si se aplica un cambio en uno de los pods este se vaya a aplicar a los otros pods. La virtud de StatefulSet es la persistencia entre las reprogramaciones de los pods.