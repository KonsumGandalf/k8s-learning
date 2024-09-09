### Long lived token
https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md

## Cli commands

### Forward dashboard pod
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443

### Generate token
kubectl -n kubernetes-dashboard create token admin-user | pbcopy