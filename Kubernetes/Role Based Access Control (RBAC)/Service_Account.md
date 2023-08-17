# Service Account
You can use below Yaml for create service account:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
```

Token is not auto-generated since v1.24 for the service account.

You should create a Secret manually:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: jenkins
  annotations:
    kubernetes.io/service-account.name: jenkins
type: kubernetes.io/service-account-token
```

To verify a secret attached to a service account as a token, use the following command:

```bash
kubectl describe sa jenkins
```

To see the generated token, use the following command:

```bash
kubectl get secret jenkins -o yaml
```

You can Copy the Token and use it to interact with API server.

We are Creating a Role for Jenkins:

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: jenkins-role
rules:
- apiGroups:
  - ""
  resources:
  - pods
  verbs:
  - create
  - list
  - update
  - get
- apiGroups:
  - apps
  resources:
  - deployments
  verbs:
  - create
  - list
  - update
```

and a role binding as well:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: jenkins-role-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: jenkins-role
subjects:
- kind: ServiceAccount
  name: jenkins
  namespace: default
```