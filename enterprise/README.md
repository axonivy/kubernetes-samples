# Axon Ivy Engine Enterprise

```bash
# deploy everything
kubectl apply -k .

# only ivy
kubectl apply -k ivy
```

## Scale Axon Ivy Engine

```bash
kubectl scale deployment ivy --replicas=3
```

## Access with port forwarding

```bash
kubectl port-forward service/ivy 8080:8080
```

Open browser http://localhost:8080
