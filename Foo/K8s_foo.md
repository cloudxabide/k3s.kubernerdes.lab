# Kubernetes Foo (K3s)

```bash
k3s check-config
```

## podCIDR
```bash
kubectl get nodes -o jsonpath='{.items[*].spec.podCIDR}'
```

## serviceSubnet
-- note: I need to find a better way to do this
```bash
SVCRANGE=$(echo '{"apiVersion":"v1","kind":"Service","metadata":{"name":"tst"},"spec":{"clusterIP":"1.1.1.1","ports":[{"port":443}]}}' | kubectl apply -f - 2>&1 | sed 's/.*valid IPs is //')
echo $SVCRANGE
```




