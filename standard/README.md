# Axon Ivy Engine Standard

```bash
# deploy everything
kubectl apply -k .

# only ivy
kubectl apply -k ivy
```

```bash
# remove everything
kubectl delete -k .
```

## Access with port forwarding

```bash
kubectl port-forward service/ivy 8080:8080
```

Open browser http://localhost:8080
