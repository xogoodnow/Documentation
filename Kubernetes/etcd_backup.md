# etcd Backup
Application data is not included in etcd.

Cluster State store in etcd.

![Untitled](../Photos/Untitled%20(10).png)

We can use this command to install etcdctl on master node: 

```bash
apt install etcd-client
```

You can check etcd certificates with this command:

```bash
cat /etc/kubernetes/manifests/etcd.yaml | grep /etc/kubernetes/pki
```

etcd backup command:

```bash
ETCDCTL_API=3 etcdctl snapshot save snapshot.file --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key
```

Check status of etcd backup:

```bash
ETCDCTL_API=3 etcdctl snapshot status snapshot.file --write-out=table
```

For restoring backup :

```bash
ETCDCTL_API=3 etcdctl snapshot restore snapshot.file --data-dir /var/lib/etcd-backup --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key
```

restore should run on all masters to effect.

we put restored file in **var/lib/etcd-backup** path, we should edit manifest of etcd.

path :

```bash
/etc/kubernetes/manifests/etcd.yaml
```

Please update the hostPath for etcd-data to reflect the new path.:

![Screenshot from 2023-07-30 16-31-23.png](../Photos/Screenshot%20from%202023-07-30%2016-31-23.png)