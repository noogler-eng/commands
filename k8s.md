# Kubernetes (K8s) Commands Reference Guide

## Cluster Management

### Cluster Information
```bash
# View cluster information
kubectl cluster-info
kubectl version
kubectl config view
kubectl get nodes
kubectl describe node <node-name>

# Switch context and namespace
kubectl config use-context <context-name>
kubectl config set-context --current --namespace=<namespace>
```

## Pod Operations

### Basic Pod Commands
```bash
# Pod management
kubectl get pods
kubectl get pods -o wide
kubectl get pods --all-namespaces
kubectl get pod <pod-name> -o yaml
kubectl describe pod <pod-name>

# Create and delete pods
kubectl run <pod-name> --image=<image-name>
kubectl delete pod <pod-name>
kubectl delete pod --all

# Execute commands in pods
kubectl exec -it <pod-name> -- /bin/bash
kubectl logs <pod-name>
kubectl logs -f <pod-name>  # Follow logs
kubectl logs --previous <pod-name>  # Previous container logs
```

## Deployment Operations

### Deployment Management
```bash
# Create and manage deployments
kubectl create deployment <name> --image=<image>
kubectl get deployments
kubectl describe deployment <name>
kubectl delete deployment <name>

# Scaling
kubectl scale deployment <name> --replicas=<count>
kubectl autoscale deployment <name> --min=2 --max=5 --cpu-percent=80

# Rolling updates
kubectl set image deployment/<name> <container>=<image>:<tag>
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name>
kubectl rollout undo deployment/<name> --to-revision=<revision>
```

## Service Operations

### Service Management
```bash
# Create and manage services
kubectl expose deployment <name> --port=<port> --type=<type>
kubectl get services
kubectl describe service <name>
kubectl delete service <name>

# Service types
# ClusterIP (default)
kubectl expose deployment <name> --port=80 --type=ClusterIP

# NodePort
kubectl expose deployment <name> --port=80 --type=NodePort

# LoadBalancer
kubectl expose deployment <name> --port=80 --type=LoadBalancer
```

## ConfigMap and Secret Operations

### ConfigMap Management
```bash
# Create and manage ConfigMaps
kubectl create configmap <name> --from-file=<path/to/file>
kubectl create configmap <name> --from-literal=key1=value1
kubectl get configmaps
kubectl describe configmap <name>
kubectl delete configmap <name>
```

### Secret Management
```bash
# Create and manage Secrets
kubectl create secret generic <name> --from-literal=key1=value1
kubectl get secrets
kubectl describe secret <name>
kubectl delete secret <name>
```

## Namespace Operations

### Namespace Management
```bash
# Create and manage namespaces
kubectl create namespace <name>
kubectl get namespaces
kubectl delete namespace <name>

# Set default namespace
kubectl config set-context --current --namespace=<name>
```

## Resource Management

### Resource Quotas and Limits
```bash
# Resource quotas
kubectl create quota <name> --hard=cpu=1,memory=1G,pods=2
kubectl get resourcequota
kubectl describe resourcequota <name>

# LimitRange
kubectl create limitrange <name> --min=cpu=0.1,memory=100Mi
kubectl get limitrange
kubectl describe limitrange <name>
```

## Storage Operations

### Persistent Volume Management
```bash
# PersistentVolume operations
kubectl get pv
kubectl describe pv <name>
kubectl delete pv <name>

# PersistentVolumeClaim operations
kubectl get pvc
kubectl describe pvc <name>
kubectl delete pvc <name>
```

## Network Operations

### Network Policy and Ingress
```bash
# Network policies
kubectl get networkpolicies
kubectl describe networkpolicy <name>

# Ingress operations
kubectl get ingress
kubectl describe ingress <name>
kubectl delete ingress <name>
```

## Advanced Operations

### Debug and Troubleshooting
```bash
# Debugging
kubectl debug node/<node-name> -it --image=ubuntu
kubectl port-forward pod/<pod-name> 8080:80
kubectl proxy
kubectl top pod
kubectl top node

# Get events
kubectl get events
kubectl get events --sort-by=.metadata.creationTimestamp
```

### Application Management
```bash
# Apply manifests
kubectl apply -f <filename.yaml>
kubectl apply -f <directory>
kubectl delete -f <filename.yaml>

# Diff changes
kubectl diff -f <filename.yaml>

# Edit resources
kubectl edit deployment <name>
kubectl edit service <name>
```

## Best Practices

1. Resource Management
   - Always set resource requests and limits
   - Use namespaces to organize resources
   - Implement network policies for security
   - Use labels and annotations effectively

2. Monitoring and Logging
   - Regularly monitor resource usage
   - Set up proper logging
   - Configure health checks
   - Use readiness and liveness probes

3. Security
   - Use RBAC for access control
   - Regularly rotate secrets
   - Keep the cluster updated
   - Implement network policies

4. Deployment Strategies
   - Use rolling updates
   - Implement proper backup strategies
   - Use ConfigMaps for configuration
   - Implement proper health checks

## Common Troubleshooting Commands

```bash
# Pod troubleshooting
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/bash
kubectl get pod <pod-name> -o yaml

# Node troubleshooting
kubectl describe node <node-name>
kubectl top node
kubectl get events

# Service troubleshooting
kubectl describe service <service-name>
kubectl get endpoints <service-name>
```

## Important Flags and Options

```bash
-n, --namespace           # Specify namespace
-o wide                  # Additional information
-o yaml                  # Output in YAML format
-o json                  # Output in JSON format
--all-namespaces        # All namespaces
-l, --selector          # Label selector
-f                      # Filename or directory
--force                 # Force operation
--dry-run=client       # Simulate operation
```
