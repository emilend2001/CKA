Question HPA
Create a new HorizontalPodAutoScaler(HPA) named apache-server in the autoscale namespace

Task
1. The HPA must target the existing deployment called apache-deployment in the autoscale namespace
2. Set the HPA to target for 50% CPU usage per Pod
3. Configure the HPA to have a minimum of 1 pod and a maximum of 4 pods
4. Set the downscale stabilization window to 30 seconds

Video Link - https://youtu.be/YGkARVFKtmM

<details><summary>Solution</summary>

```bash
Create HPA for apache-deployment in autoscale namespace
kubectl get deploy -n autoscale
cat <<'EOF' > hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: apache-server
  namespace: autoscale
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: apache-deployment
  minReplicas: 1
  maxReplicas: 4
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 30
EOF
kubectl apply -f hpa.yaml
kubectl get hpa -n autoscale
```

</details>
