Question:
After a cluster migration, the controlplane kube-apiserver is not coming up
Before the migration, the etcd was external and in HA, after migration the kube-api server was pointing to etcd peer port 2380

Task
Fix it

Video Link - https://youtu.be/IL448T6r8H4

<details><summary>Solution</summary>

```bash
Check apiserver logs, fix etcd endpoint
journalctl -u kube-apiserver | tail
sudo sed -n '1,200p' /etc/kubernetes/manifests/kube-apiserver.yaml
Ensure flag uses correct etcd port
--etcd-servers=https://127.0.0.1:2379
sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml   # correct port/IP, save and wait for static pod restart

If scheduler also broken, verify its flags
kubectl -n kube-system get pods | grep kube-scheduler
sudo vi /etc/kubernetes/manifests/kube-scheduler.yaml   # kubeconfig path, API server endpoint, cert refs
```

</details>
