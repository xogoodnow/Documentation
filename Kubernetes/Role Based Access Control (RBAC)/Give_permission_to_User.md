# Give permission to User
## Give the user ClusterRole and bind it

you can check API groups below link:

https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.27/

You can use * to specify all resources or verbs to Kubernetes.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cr-farshid
rules:
- apiGroups:
  - ""         # refers to core API groups
  resources: ["*"]
  verbs: ["*"]
- apiGroups:
  - apps
  resources:
  - deployments
  verbs:
  - create
  - list
  - update
```

 after apply , you can check it :

```bash
 kubectl get clusterrole
```

for human readable permission :

```bash
kubectl describe clusterrole cr-farshid
```

## ClusterRole Binding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: crb-farshid
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cr-farshid
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: farshid
```

for human readable permission :

```bash
kubectl describe clusterrolebinding cr-farshid
```