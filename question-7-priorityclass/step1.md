Question
You're working in a kubernetes cluster with an existing deployment named busybox-logger running
in the priority namespace.
The cluster already has at least one user defined Priority Class

Tasks:
1. Create a new Priority Class named high-priority for user workloads. The value of this class should
be exactly one less than the highest existing user-defined priority class
2. Patch the existing deployment busybox-logger in the priority namespace to use the newly created
high-priority class

Video Link - https://youtu.be/CZzxGyF6OHc

<details><summary>Solution</summary>

```bash
Create PriorityClass just below existing max (e.g., 999)
kubectl create priorityclass high-priority --value=999 --description="high priority"
kubectl get pc

Patch deployment to use it
kubectl patch deployment busybox-logger -n priority -p '{"spec":{"template":{"spec":{"priorityClassName":"high-priority"}}}}'
kubectl describe deployment busybox-logger -n priority | grep -i "Priority Class"
```

</details>
