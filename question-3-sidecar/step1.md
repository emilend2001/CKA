Question SideCar

Task
Update the existing wordpress deployment adding a sidecar container named sidecar using the busybox:stable
image to the existing pod
The new sidecar container has to run the following command
"/bin/sh -c tail -f /var/log/wordpress.log"
Use a volume mounted at /var/log to make the log file wordpress.log available to the co-located container

Video link - https://youtu.be/3xraEGGQJDY

<details><summary>Solution</summary>

```bash
Patch wordpress deployment to add shared volume + sidecar
cat <<'EOF' | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  template:
    spec:
      volumes:
      - name: log
        emptyDir: {}
      containers:
      - name: wordpress
        volumeMounts:
        - name: log
          mountPath: /var/log
      - name: sidecar
        image: busybox:stable
        command: ["/bin/sh","-c","tail -f /var/log/wordpress.log"]
        volumeMounts:
        - name: log
          mountPath: /var/log
EOF

kubectl rollout status deployment wordpress
kubectl get pods -l app=wordpress
```

</details>
