## Online Examples
- Guide https://strimzi.io/quickstarts/
- Kraft Examples https://github.com/strimzi/strimzi-kafka-operator/blob/0.43.0/examples/kafka/kraft

## CLI 
1. Create Namespace
kubectl create namespace kafka
2. Setup cluster
kubectl apply -f https://strimzi.io/examples/latest/kafka/kraft/kafka-ephemeral.yaml -n kafka 
3. Create consumer pod
kubectl -n kafka run kafka-consumer -ti --image=quay.io/strimzi/kafka:0.43.0-kafka-3.8.0 --rm=true --restart=Never -- bin/kafka-console-consumer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic --from-beginning
4. Create producer pod
kubectl -n kafka run kafka-producer -ti --image=quay.io/strimzi/kafka:0.43.0-kafka-3.8.0 --rm=true --restart=Never -- bin/kafka-console-producer.sh --bootstrap-server my-cluster-kafka-bootstrap:9092 --topic my-topic
