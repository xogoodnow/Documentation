# Role Based Access Control (RBAC)

![Untitled](../../Photos/Untitled%20(11).png)

You check enabled authorization modules in :

```bash
/etc/kubernetes/manifests/kube-apiserver.yaml
```

# Role

- With Role component, you can define namespaced permissions.
- Bound to a specific namespace
- What resources in that namespace you can access?
    - example
        - “pod”
        - “Deployment”
        - “Service”
        - …`
- What action you can do with this resource?
    - example
        - “list”
        - “get”
        - “update”
        - “delete”
        - …

**In a specific namespace, you have the ability to set user permissions and determine what actions they can take.**

for **core** API group, we leave it empty:

**apiGroups**: [””]

you can check API groups below link:

https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.27/

We can specify which user or group has access to a single pod using **resourceName** attribute.

![Untitled](../../Photos/Untitled%20(12).png)

# Role Binding

We create various roles and link them to users or groups using role Binding.

# Cluster Role

What about k8s administrators? They are performing a **cluster-wide** operation, rather than targeting a specific namespace.

We can determine whether the admins have access to view nodes or not.

Can they create nodes?

Can they delete nodes? 

We can define CR(cluster Role) and attach the cluster role to the admin group using **ClusterRoleBinding**.

![Untitled](../../Photos/Untitled%20(13).png)

You can specify ClusterRole for namespace management.

![Untitled](../../Photos/Untitled%20(14).png)

ClusterRoleBinding

![Untitled](../../Photos/Untitled%20(15).png)

# **How do we create Users and Groups?**

Kubernetes doesn't manage users natively.

**Admins** can choose from different authentication strategies.

No k8s objects exist for representing **normal user** accounts.

So k8s relies on external sources for creating and managing users and groups. The external sources for authentication could be static files with username, uid, and token.

It could be also a certificate that k8s signed itself.

or It can be 3rd party identity service like LDAP.

Kubernetes administrator chooses one of these external sources.

The **API server** attempts to authenticate and authorize incoming requests. You can pass external configurations to the API server to use it to perform its tasks.

# ServiceAccount

Applications or services also need to access the cluster.

for example CI/CD tools.

The K8s component known as ServiceAccount represents a user for an application.

For example for the services like Prometheus, Jenkins get a service account.

You can Link a Service Account to a Role with Role Binding.

# Checking API Access

kubectl provides a auth can-i subcommand, to quickly check if current user can perform a given action.

```bash
kubectl auth can-i create deployments -n namespace
```

# Configure User Authentication Using certificates:

[Configure User Authentication Using Certificates](https://www.notion.so/Configure-User-Authentication-Using-Certificates-b96f631f3d5e4dfa89d964048cf72858?pvs=21)

# Give permission to User

[Give permission to User](https://www.notion.so/Give-permission-to-User-1ec7b3d2c64c47ec855734cc3afa4364?pvs=21)

# Service Account for none human users.

[Service Account](https://www.notion.so/Service-Account-4053b4122f9c4720990b777c09dcd7c1?pvs=21)