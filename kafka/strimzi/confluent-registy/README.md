# Confluence Schema Registry
This order is dedicated to deploy a confluent schema registry for the strimzi kafka cluster in the kafka namespace

# Deploy via Helm
```shell
helm install \
  confluent-schema-registry \
  oci://registry-1.docker.io/bitnamicharts/schema-registry \
  --install \
  --namespace=kafka \
  --values=values.yaml
```

# Expose the service
Expose the kubernetes service via LoadBalancer
```shell
kubectl apply -f ./service.yaml
```