Question Storage Class

Tasks
1. Create a new StorageClass named local-storage with the provisioner rancher.io/local-path. Set
the VolumeBindingMode to WaitForFirstCustomer. Do not make the SC default
2. Patch the StorageClass to make it the default StorageClass
3. Ensure local-storage is the only default class
Do not modify any existing Deployments or PersistentVolumeClaims

Video link - https://youtu.be/di7X7OHn2fc

<details><summary>Solution</summary>

```bash
Create StorageClass
cat <<'EOF' > sc.yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
  annotations:
    storageclass.kubernetes.io/is-default-class: "false"
provisioner: rancher.io/local-path
volumeBindingMode: WaitForFirstConsumer
EOF
kubectl apply -f sc.yaml
kubectl get sc

Make it default, remove default from local-path
kubectl patch storageclass local-storage -p '{"metadata":{"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
kubectl patch storageclass local-path -p '{"metadata":{"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'
kubectl get sc
```

</details>
