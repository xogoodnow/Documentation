# Access REST API 

## Access REST API with kubectl proxy

This command will reverse proxy the API request from port 8080 :

```bash
kubectl proxy --port=8080 &
```

Then we can test it :

```bash
curl http://localhost:8080/api
```

or :

```bash
curl http://localhost:8080/api/v1
```

Then you can kill The process using **kill -9** command

## Access REST API with kubectl without proxy

Create a service account:

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
```

You can then create a token for the service account.

```bash
kubectl create token <ServiceAccountName>
```

To check access, you can use the curl command:

```bash
curl -X GET https://5.34.201.65:6443/api -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6IkF2aERsWWpwbFpBeGNzQ0pKNk1GcHR5UnhhYkZQZnJtMnltZjZGNVdfNDAifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjkxMjI2OTkxLCJpYXQiOjE2OTEyMjMzOTEsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJkZWZhdWx0Iiwic2VydmljZWFjY291bnQiOnsibmFtZSI6ImplbmtpbnMiLCJ1aWQiOiI2YWI4OTk0Ny0xMjlkLTRjMGItYTQ4MS1hN2VlYjllZTVlMmMifX0sIm5iZiI6MTY5MTIyMzM5MSwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRlZmF1bHQ6amVua2lucyJ9.Na49ayyeuodJ8eunLxFa3wEL3TdzMlyb4EKc_Y3ICN_kIKKM1VYE_hbrPnQCaJlRb-uLXzgp5NuFEqgtJkWLAfJpBKykxuOvvO7TcrdLaf7eISRUX50l4Ird6SqGITfGXTt1LZg1vxukSZXyJgQPuI3jDqL7H6xcFslLG8tOE1E4SpmPqX3YGGYZmjrFeTasXrqNnMJPbqB5Fh2uH-sC0VI6EeDC26uj5hughr0dSfKvpt7Xz_Kb4boYd8W-wdQ6QK-poYmJKNoJFmC7e8zp5v7L3etdIGZPQ83YDoHjxUuF6AEAdR-185y75mNH1AXd5l7hZKrQ0S-ALORpINvjTA"
```

Please check the API samples in the following link:

https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.27/#create-daemonset-v1-apps

Below is a link to some supported samples in different programming languages that Kubernetes supports:

https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/