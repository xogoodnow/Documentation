# Taint & toleration
**It is possible to define node specifications, including which pods can be scheduled on specific nodes.**

## Taint:

Taint can prevent pods with specific labels from being scheduled on a certain node.

Taint applies to nodes.

To check which nodes have a taint, use the following command:

```bash
kubectl describe node | grep Taint
```

Who set Taint on master nodes? answer is kubeadm

## Toleration:

Toleration applies to pods.

It allows the pods to schedule on nodes with matching taints.

This deployment allow the pod to be schedule on master nodes:
Keep in mind that these tolerations only work on "noschedule" and "noexecute" taints.

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
      tolerations:
      - effect: NoExecute
        operator: Exists
      nodeName: master-1
```



