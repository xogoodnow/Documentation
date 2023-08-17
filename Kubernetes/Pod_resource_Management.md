# Pod resource Management

It is important to establish a request and limit for resources in pods since they are initially set to have access to all resources on a server. 

The "request" refers to the minimum amount of resources a pod can use, while the "limit" is the maximum amount it can use. There may be instances where a pod requires additional resources, such as during a period of high demand lasting an hour.

# How to check which pods have resource requests and limits set?

```bash
kubectl get pod -o jsonpath="{range .items[*]} {.metadata.name}{.spec.containers[*].resources}{'\n'}"
```

**Deployment**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:stable
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "64Mi"   #Mi: mebibyte
            cpu: "250m"      #milicore
          limits:
            memory: "128Mi"   #Mi: mebibyte
            cpu: "500m"       #milicore
```