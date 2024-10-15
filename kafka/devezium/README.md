# What is Debezium?
Its a connector for kafka which allows to watch and react to db modifications. It relies on the change data capture technique.

# Requirements
Based on the strimzi kafka cluster deployment

Create push certificate for registry (docker io): 
```
  kubectl create secret docker-registry my-secret  --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
```

# Deployment

## Build & Deploy Connect
Builds Debezium connector with postgres & mysql plugins.
Replace in `bootstrapServers` the address with the one of the ip of the own cluster service.

```shell
kubectl apply -f ./db-connect.yaml
```

## Deploy MySQL Connector
Replace `database.hostname` with the name of the db service on your device
Replace `labels: strimzi.io/cluster` with the name of the db.connect.yaml.

```shell
kubectl apply -f ./connector-mysql.yaml
```

## Deploy Postgres Connector
```shell
kubectl apply -f ./connector-mysql.yaml
```

# Verify 
By running a watcher for events its possible to verify if the sink or source run correctly

```shell
# MySQL
kubectl run -n kafka -it --rm --image=quay.io/debezium/tooling:1.2 --restart=Never watcher -- kcat -b my-cluster-kafka-bootstrap:9092 -C -o beginning -t mysql.inventory.customers
# Postgres - Seems to have a bug!!!
kubectl run -n kafka -it --rm --image=quay.io/debezium/tooling:1.2 --restart=Never watcher-postgres -- kcat -b my-cluster-kafka-bootstrap:9092 -C -o beginning -t postgres.customers
```