# Axon Ivy Engine Enterprise

Create an own namespaces call `sandbox` to make this example work. See
[here](../README.md) how you can create a namespace and connect to it with
`kubectl`.

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
