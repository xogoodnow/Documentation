# Define specific node(s) for Pod
This is typically used for pods that require high amounts of data and CPU processing. These pods will have access to all resources on the node. 

- It is important to note that only one replica of the pod should run on each node.

The scheduler is intelligent and can determine which node has the most capacity and is least busy to run a pod.

At times, we may have to manually select the location where the pod should be scheduled by the scheduler.

## First way :

A variable named nodeName can be defined for it. you can use it in your deployment. 

> For example : 
        nodeName: worker-3
> 

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
      nodeSelector: 
        type: most-cpu
```

## Second Way:

We can attach labels to nodes to help us when the name of a node changes, ensuring that nothing goes wrong.

command to check nodes labels:

```bash
kubectl get node --show-labels
```

Set label for a node :

```bash
kubectl label node worker-2 <LabelKey>=<Value>
```

for example: type=most-cpu

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
      nodeSelector: 
        type: cpu
```

it will schedule pods on nodes that have “type: cpu”

 

## Third way: (recommended)

- **What happens if a node doesn't have the resources to schedule a pod?**
- **What if we have tens of nodes across different regions with varying operating systems?**

The phrase "requiredDuringSchedulingIgnoredDuringExecution" indicates that the scheduler will **must** schedule pod on a node that has a specific label or set of labels.

When scheduling a pod, the label "preferredDuringSchedulingIgnoredDuringExecution" indicates that I prefer the node with this label. If that node with this label has enough resources, it will be selected.

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
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: kubernetes.io/os
                operator: In
                values:
                - linux
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 1
            preference:
              matchExpressions:
              - key: type 
                operator: In
                values:
                - most-cpu
```

We other operators to specify which node the scheduler should not schedule.

Below is a list of operators that you can view:

![Screenshot from 2023-07-29 00-30-19.png](../Photos/Screenshot%20from%202023-07-29%2000-30-19.png)