# Liveness probe & Readiness probe
Kubernetes knows the pod state, but doesn't know state of application inside.

Liveness prob for application state 

Health check only after container started.

There are three way to have a liveness prob:

1. Exec Probes: kubelet executes the specified command to check the health.
2. TCP probes: Kubelet makes probe connection at the node, not in the pod.
3. HTTP probes: kubelet sends an HTTP request to specified path and port.

![Untitled](../Photos/Untitled%20(5).png)

What about the starting up process? answer is Readiness probe

- configuration very similar to liveness probe
- both check application availibily

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
        readinessProbe:
          tcpSocket:
            port: 80
          initialDelaySeconds: 10 # tell kubelet that it should wait 10 seconds before performing the first probe.
          periodSeconds: 5  # make sure that the application it realy started to send traffic to it . it will check in period 5 second

        livenessProbe:
          tcpSocket: 
            port: 80
          initialDelaySeconds: 5   #after 5 second application started, liveness will start 
          periodSeconds: 15      # it will check every 15 second
```