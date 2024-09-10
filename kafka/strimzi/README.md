## Online Examples
- Guide https://strimzi.io/quickstarts/
- Kraft Examples https://github.com/strimzi/strimzi-kafka-operator/blob/0.43.0/examples/kafka/kraft

## CLI 
1. Create Namespace
kubectl create namespace kafka
2. Label nodes to allow rack spreading
kubectl label nodes konsum-gandalf-b760-pro-rs topology.kubernetes.io/zone=rack-0
kubectl label nodes worker-1 topology.kubernetes.io/zone=rack-1
kubectl label nodes worker-2 topology.kubernetes.io/zone=rack-2
2. Setup cluster
kubectl apply -f https://strimzi.io/examples/latest/kafka/kraft/kafka-ephemeral.yaml -n kafka 
=> Watch what is happening: kubectl get pod -n kafka --watch 
4. Create consumer pod
kubectl -n kafka run kafka-consumer -ti --image=quay.io/strimzi/kafka:0.43.0-kafka-3.8.0 --rm=true --restart=Never -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic --from-beginning
5. Create producer pod
kubectl -n kafka run kafka-producer -ti --image=quay.io/strimzi/kafka:0.43.0-kafka-3.8.0 --rm=true --restart=Never -- bin/kafka-console-producer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic

---
5. Connect later to consumer pod
kubectl exec -n kafka kafka-consumer -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic --from-beginning
6. Connect later to producer pod
kubectl exec -n kafka kafka-producer -- bin/kafka-console-producer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic

## Setup External Access
```YAML
    - name: external
      port: 9094
      type: loadbalancer
      tls: false
```
kubectl apply -f ./kafka-ephemeral-external.yaml -n kafka 

Then use the external IP with port f.e. 192.168.4.16:9094

/opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 192.168.4.16:9094 --topic my-topic --from-beginning