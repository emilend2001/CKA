Question:
There are two deployments, Frontend and Backend
Frontend is in the frontend namespace, Backend is in the backend namespace

Task
Look at the Network Policy YAML files in /root/network-policies
Decide which of the policies provides the functionality to allow interaction between the
frontend and the backend deployments in the least permissive way and deploy that yaml

Video Link - https://youtu.be/rA8mXYTU0W8

<details><summary>Solution</summary>

```bash
Compare provided policies and pick the least permissive that matches requirements
cat /root/network-policies/network-policy-1.yaml   # allows all ingress (too open)
cat /root/network-policies/network-policy-2.yaml   # extra IP allowed (too open)
cat /root/network-policies/network-policy-3.yaml   # only frontend namespace/pods allowed
kubectl get pods -n frontend --show-labels         # confirm app=frontend label
kubectl apply -f /root/network-policies/network-policy-3.yaml
```

</details>
