# Network Policies
For security reasons, it is recommended that you create a network policy that outlines which pods are allowed to interact with each other.

Example for ingress:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: mysql
  namespace: default
spec:
  podSelector:
    matchLabels:
      apps: mysql
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: backend
# only apps with backend label can intract with mysql
    ports:
    - protocol: TCP
      port: 3306
# and backend can only intract with port 3306

#another condition
  - from:
    - podSelector:
        matchLabels:
          app: phpmyadmin
# only apps with phpmyadmin label can intract with mysql
    ports:
    - protocol: TCP
      port: 3306
# and phpmyadmin can only intract with port 3306
```

Example for egress :

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend
  namespace: default
spec:
  podSelector:
    matchLabels:
      apps: backend
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: mysql
    ports:
    - protocol: TCP
      port: 3306
  - to:
    - podSelector:
        matchLabels:
          app: redis
    ports:
    - protocol: TCP
      port: 6379 # Redis
# Enable backend pods to only interact with the MySQL and Redis applications on port 3306 , port 6379
```

Namespaces can have labels too.

If the destination pod is in a different namespace you can define namespaceSelector for each role. you can use