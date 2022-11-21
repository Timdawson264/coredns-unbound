

### Using the recursive resolver in openshift


```
kubectl create ns recursive-dns
kubectl apply -n recursive-dns -f ./deployment.yaml

env DNS_IP=$(kubectl get svc -n recursive-dns rdns -ojsonpath="{.spec.clusterIP}")  yq  -o json '.[].value.address=strenv(DNS_IP)'  ./dns-patch.yaml | kubectl patch --patch-file=/dev/stdin DNS.operator.openshift.io default --type=json
```