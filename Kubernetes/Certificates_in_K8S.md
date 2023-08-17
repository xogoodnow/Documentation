# Certificates in K8S
You can see auto-generated certificate with kubeadm in: 

```bash
/etc/kubernetes/pki
```

kubeadm also generates client certificate for services talking to the API server. You can see the private and public key in Kube config file.

### Who signed those Certificates?

Kubeadm generated a CA for k8s cluster.

### Why are clients trusting an auto-generated CA?

Because all clients have a copy of k8s CA.

**ca.crt is a certificate authority that signs all others certificates.**

```bash
/etc/kubernetes/pki/ca.crt
```

# Check Certificate Expiration

You can check the expiration time of the certificates:

```bash
kubeadm certs check-expiration
```

or 

```bash
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

**Upgrading the cluster result in the renewing the certificates.**

If you frequently upgrade your cluster, there's no need to manually renew certificates as kubeadm will handle it for you.

## Renew certificate manualy

You can renew certificate manually using 

```bash
kubeadm cert renew <component>
```