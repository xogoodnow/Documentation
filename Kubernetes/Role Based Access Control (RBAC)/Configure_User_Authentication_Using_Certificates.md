# Configure User Authentication Using Certificates
## Process of signing a Client Certificate:

1. Create Key-Pair
2. Generate Certificate Signing Request (CSR)
3. Send CSR using k8s Certificate API
4. k8s signs the certificate for you
5. k8s admin approves the certificate.

### Generate Private Key & CSR

Request for a private key :

```bash
openssl genrsa -out farshid.key 2048
```

Create a CSR :

```bash
openssl req -new -key farshid.key -subj "/CN=farshid" -out farshid.csr
```

Create CSR k8s Resource: (we are sending this CSR to Kubernetes)

- To proceed, please encode the value of your previously created CSR file using base64. Then, insert the encoded value into the YAML file provided below.
    
    ```bash
    cat farshid.csr | base64 | tr -d "\n”
    ```
    

```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: farshid
spec:
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBNQTBHQTFVRUF3d0dZVzVuWld4aE1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJDZ0tDQVFFQTByczhJTHRHdTYxakx2dHhWTTJSVlRWMDNHWlJTWWw0dWluVWo4RElaWjBOCnR2MUZtRVFSd3VoaUZsOFEzcWl0Qm0wMUFSMkNJVXBGd2ZzSjZ4MXF3ckJzVkhZbGlBNVhwRVpZM3ExcGswSDQKM3Z3aGJlK1o2MVNrVHF5SVBYUUwrTWM5T1Nsbm0xb0R2N0NtSkZNMUlMRVI3QTVGZnZKOEdFRjJ6dHBoaUlFMwpub1dtdHNZb3JuT2wzc2lHQ2ZGZzR4Zmd4eW8ybmlneFNVekl1bXNnVm9PM2ttT0x1RVF6cXpkakJ3TFJXbWlECklmMXBMWnoyalVnald4UkhCM1gyWnVVV1d1T09PZnpXM01LaE8ybHEvZi9DdS8wYk83c0x0MCt3U2ZMSU91TFcKcW90blZtRmxMMytqTy82WDNDKzBERHk5aUtwbXJjVDBnWGZLemE1dHJRSURBUUFCb0FBd0RRWUpLb1pJaHZjTgpBUUVMQlFBRGdnRUJBR05WdmVIOGR4ZzNvK21VeVRkbmFjVmQ1N24zSkExdnZEU1JWREkyQTZ1eXN3ZFp1L1BVCkkwZXpZWFV0RVNnSk1IRmQycVVNMjNuNVJsSXJ3R0xuUXFISUh5VStWWHhsdnZsRnpNOVpEWllSTmU3QlJvYXgKQVlEdUI5STZXT3FYbkFvczFqRmxNUG5NbFpqdU5kSGxpT1BjTU1oNndLaTZzZFhpVStHYTJ2RUVLY01jSVUyRgpvU2djUWdMYTk0aEpacGk3ZnNMdm1OQUxoT045UHdNMGM1dVJVejV4T0dGMUtCbWRSeEgvbUNOS2JKYjFRQm1HCkkwYitEUEdaTktXTU0xMzhIQXdoV0tkNjVoVHdYOWl4V3ZHMkh4TG1WQzg0L1BHT0tWQW9FNkpsYWFHdTlQVmkKdjlOSjVaZlZrcXdCd0hKbzZXdk9xVlA3SVFjZmg3d0drWm89Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400000
  usages:
  - client auth
```

You can also change expirationSeconds and name.

and then apply it.

```bash
kubectl apply -f farshid-csr.yaml
```

Now your CSR is in pending state :

```bash
kubectl get csr 
```

For approve it, you can use this command:

```bash
kubectl certificate approve farshid -n rb
```

Once your request is approved, you can edit CSR and check the status in the certificate. can copy that and decode it to see real certificate :

```bash
echo "VALUE of certicate on base64" | base64 --decode > farshid.crt
```

now we have a key pair of public and private key.

## Connect to K8S With mentioned keys

To Find an API server address:

```bash
kubectl cluster-info
```

and you can create a config file with sample:

```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURCVENDQWUyZ0F3SUJBZ0lJSnBtL1FhTXZ2L0l3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TXpBNE1ESXhOREkzTWpsYUZ3MHpNekEzTXpBeE5ESTNNamxhTUJVeApFekFSQmdOVkJBTVRDbXQxWW1WeWJtVjBaWE13Z2dFaU1BMEdDU3FHU0liM0RRRUJBUVVBQTRJQkR3QXdnZ0VLCkFvSUJBUURLWUFwRnlJdVE2azdCZ3VUK09YcDZwQTdUYStyb2VWWU92N0lJMzV3Z2J5Q0ZaeGlCeVprUTE1bUoKcEc2OVg3cUN4ZkxSTXhCWXd3ZU9JcThLclc1T0x5U1JCRk1Jb0c3OW1Yc0x1NmF6L1h5NTVhaU1TUmhSUzBQTgpDcUFISlFWcW9lU29Wcmd5MXcyVGtGc0dOWGJXa3Jud0c3OGl6ZEM1bkcwdzdzNERhbDdEM05SQnN4ZHlFS0JVCnhPWEFQYTlPNkRCN3RaZnhKUitERHJGSmhFOVFrRU1HeGorNFYrN1djaE1mYzJLOFZ2Ylc0NTlTcWZCUW4wa0kKOVpkOEV0RjRabGU1cHJ3TmdwbGlXU21BOVQ3cUpUT2d5Nkh5bE5QTGVtM2JCMGR0YmdLL1o1M2d6TEMySWxlbQpKZlBqSkY5clBDczBiZjRqL1htQWdwamkwOVNkQWdNQkFBR2pXVEJYTUE0R0ExVWREd0VCL3dRRUF3SUNwREFQCkJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJSU2xOOVJ5YmQ5ZjNIUU5oVitWZHYwbzJiY0N6QVYKQmdOVkhSRUVEakFNZ2dwcmRXSmxjbTVsZEdWek1BMEdDU3FHU0liM0RRRUJDd1VBQTRJQkFRQWpuQ3pDQTRGWgo3QzhZWThiSGhBaGc1RS9GbUNPKy9rdWJidlcxb1RZT2hub1l1b1FJY3dxOERlQVFwYTRNWTZGOEpMbEtDekNoClJ1UmlKZDVzbzFhUTVuZzJLdDNDR0ttRFFlNXRoRGpzT0lpRXVTRmtrWTB2YVhISjZsMlFGbGNLeDJoUzVnaXQKSXJLZHp1WVN4bzdRZ1VsZDFERWZFcHUvank5Qzkxdk9wYjV2UUc0eDhUNVlyN0VldjUxSDRXVjliOTBuWU5MVwpxVjcxYVNhSjI5ZklIcGtkczI2ejdFYWlFU1pyZFBwdk10ZjNVZnUyNHAwYkNKM1lZM3psSXBJZ3NTRFJMZkxkCkwrd013TGlLWElPa2w4T0dqWjB6dHUyMjJOSTNFbmpnWDJtV0NaOFFMa1MxalJGTDE4cnZYUjZqY0w1R2M3bWkKL3dmQlM2cWUxS1ZjCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    server: https://5.34.201.65:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: farshid
  name: farshid@kubernetes
current-context: farshid@kubernetes
kind: Config
preferences: {}
users:
- name: farshid
  user:
    client-certificate: ./farshid.crt
    client-key: ./farshid.key
```

You can find certificate-authority-data in admin kube config file or API server manifest.

You can put config file in **~/.kube/config** path.