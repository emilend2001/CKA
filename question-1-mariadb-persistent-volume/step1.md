Question:
A user accidentally deleted the MariaDB Deployment in the mariadb namespace. The deployment
was configured with persistent storage. Your responsibility is to re-establish the deployment
while ensuring data is preserved by reusing the available PersistentVolume

Task
A PersistentVolume already exists and is retained for reuse. Only one PV exists
Create a Persistent Volume Claim (PVC) named mariadb in the mariadb namespace with the spec
Access Mode = ReadWriteOnce
Storage = 250Mi
Edit the MariaDb Deployment file located at ~/mariadb-deploy.yaml to use the PVC created in the previous step
Apply the updated Deployment file to the cluster
Ensure the MariaDB Deployment is running and Stable

Video Link - https://youtu.be/aXvvc1EB1zg

<details><summary>Solution</summary>

```bash
Step 1: create PVC with no storageClass (PV is pre-reset by LabSetUp.bash)
cat <<'EOF' > pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mariadb
  namespace: mariadb
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
EOF
kubectl apply -f pvc.yaml
kubectl get pvc mariadb -n mariadb
kubectl get pv mariadb-pv     # should show Bound to mariadb

Step 2: ensure deployment uses the PVC
mariadb-deploy.yaml should mount claimName: mariadb
(LabSetUp.bash leaves claimName blank for practice)
kubectl apply -f mariadb-deploy.yaml
kubectl get pods -n mariadb
```

</details>
