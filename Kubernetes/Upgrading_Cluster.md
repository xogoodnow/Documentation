# Upgrading Cluster
## Upgrade Control-plane

CHECK THIS LINK :

https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

We are starting the upgrade process from the master nodes. 

- Upgrading the master node has no effect on worker nodes and **does not cause any application downtime.**
- While upgrading master nodes, certain management functions may become unavailable and **any crashed pods will not be automatically rescheduled**.
- If we have a multi-master cluster, we will upgrade the master nodes one by one.
1. To begin, we need to upgrade Kubeadm to the required version for upgrading the cluster.
    
    You can see the available versions of kubeadm:
    
    ```bash
    apt-cache madison kubeadm 
    ```
    
    Current kubeadm version :
    
    ```bash
    kubeadm version
    ```
    
2. After obtaining the latest version of kubeadm, we can proceed with upgrading all control plane components and renewing the cluster certificate.
    
    ```bash
    kubeadm upgrade apply 1.27.0
    ```
    
3. Then We should remove pods that running on that node. To remove all pods running on a certain node, we need to drain that node:
    
    ```bash
    kubectl drain master-1
    ```
    
    - It will evict all pods safely.
    - It will be marked unschedulable
4. **Kubeadm doesn't upgrade kubelet and kubectl, we should upgrade them separately.**
5. Change master back to be schedulable.
    
    ```bash
    kubectl uncordon master-1
    ```
    

## Upgrade Worker nodes

Once all master nodes upgraded, we will need to upgrade the worker node one by one.

1. Please upgrade kubeadm, as it will also upgrade all kubelet configuration.
    
    ```bash
    kubeadm upgrade apply 1.27.0
    ```
    
2. Execute kubeadm to upgrade all kubelet configuration.
3. Drain the node(Pods will reschedule on other nodes)
    
    ```bash
    kubectl drain worker-1
    ```
    
4. Upgrade kubelet.
5. Change worker Node back to schedulable.
    
    This command will put the node in maintenance mode.
    
    ```bash
    kubectl cordon <node>
    ```
    
    To reschedule, use this command for the node.
    
    ```bash
    kubectl uncordon <node>
    ```

