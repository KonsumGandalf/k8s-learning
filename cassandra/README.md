# Cassandra 

## Spinning up deployment
Either deploy the arm or amd deployment based on your architecture.

```shell
kubectl apply -f ./deployment.yaml
```

## Forwarding the service to internal services
kubectl port-forward -n cassandra svc/cassandra 9042:9042

microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443