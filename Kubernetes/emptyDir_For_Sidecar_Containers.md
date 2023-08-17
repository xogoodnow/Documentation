# emptyDir For Sidecar Containers

You can see the deployment on [github](https://github.com/farshidrahimi/kubernetes/blob/main/Deployments/nginx_with_emptyDir_deployment.yaml)

![Screenshot from 2023-07-28 22-18-20.png](../Photos/Screenshot%20from%202023-07-28%2022-18-20.png)

Deployment sample:
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
        command: ['sh', '-c']
        args:
        - while true ; do
            echo "$(date) INFO some app data" >> /var/log/myapp.log;
            sleep 5;
          done
    
        volumeMounts:
        - name: log-data
          mountPath: /var/log

      - name: log-sidecar
        image: busybox
        command: ['sh', '-c']
        args:
        - tail -f /var/log/myapp.log
        volumeMounts:
        - name: log-data
          mountPath: /var/log

      volumes:
      - name: log-data
        emptyDir: {}
```