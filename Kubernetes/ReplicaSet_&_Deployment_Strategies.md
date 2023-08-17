# ReplicaSet & Deployment Strategies
When you apply deployment, a **ReplicaSet** is automatically created in the background.

**Deployment** Creates an application rollout.

![Untitled](../Photos/Untitled%20(6).png)

## Recreate strategy:

When we update the deployment, a new **Replicaset** will be created while the old Replicaset remains to remove any existing resources from the old deployment.

This strategy includes Application downtime through removing old replicaset and creating new ones.

## **Rolling update strategy:**

The Deployment updates Pods in a rolling update fashion.

For instance, it will first create a pod, then remove the old pod, followed by creating the second pod and removing the second one.

You can check the rolling update strategy in deployment.

![Untitled](../Photos/Untitled%20(7).png)

maxSurge: specifies the max number of pods that can be created over the desired number of pods.

MaxUnavailable: Specifies the max number of Pods that can be unavailable during the update process.

the first thing we can do with “kubectl rollout” is that we can restart deployment:

![Untitled](../Photos/Untitled%20(8).png)

Command:

```bash
kubectl rollout restart deployment/<name> -n test
```

You can see the rollout history command :

```bash
kubectl rollout history deployment nginx-deployment -n test
```

You can roll back to previous revision using :

```bash
kubectl rollout undo deployment nginx-deployment -n test
```