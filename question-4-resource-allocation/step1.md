Question
You are managing a WordPress application running in a Kubernetes cluster
Your task is to adjust the Pod resource requests and limits to ensure stable operation

Tasks
1. Scale down the wordpress deployment to 0 replicas
2. Edit the deployment and divide the node resource evenly across all 3 pods
3. Assign fair and equal CPU and memory to each Pod
4. Add sufficient overhead to avoid node instability
Ensure both the init containers and the main containers use exactly the same resource requests and limits
After making the changes scale the deployment back to 3 replicas

Video link - https://youtu.be/ZqGDdETii8c

<details><summary>Solution</summary>

```bash
Step 1: pause workload
kubectl scale deployment wordpress --replicas 0

Step 2: edit deployment (set same resources on all init + main containers)
kubectl edit deployment wordpress
In spec.template.spec.containers[] and spec.template.spec.initContainers[] set:
resources:
requests:
cpu: "300m"
memory: "600Mi"
limits:
cpu: "400m"
memory: "700Mi"
(Values are just an example of dividing the node evenly and keeping some headroom;
ensure every container—init and main—uses the exact same requests/limits.)

Step 3: resume replicas
kubectl scale deployment wordpress --replicas 3
kubectl rollout status deployment wordpress
kubectl get pods -l app=wordpress
```

</details>
